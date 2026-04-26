# Threat model — v0.1

> **Version:** v0.1 (BUILD-PLAN session 4, 2026-04-26).
> **Scope:** the planned Superglue Pro CLI, output schema, MCP server,
> pattern library, fixture corpus, build / release pipeline, and
> distribution paths. Many surfaces don't exist yet at the time of writing
> (`v0.0.1-alpha` ships in BUILD-PLAN session 14); each is marked
> **planned** with the BUILD-PLAN session that delivers it.
> **Methodology:** STRIDE-lite per surface — pragmatic, not exhaustive.
> Updated whenever a new surface lands or an architectural shift happens.
> **Reporting a vulnerability:** see [`SECURITY.md`](SECURITY.md).

## Assumed adversaries

| Adversary | Description |
|---|---|
| **Opportunistic mass attacker** | Crawls public repos for credentials, vulnerable patterns, supply-chain entry points. Default-on for any OSS project. |
| **Targeted attacker against a downstream consumer** | Wants to ship a malicious pattern, fixture, dependency update, or release artefact to harm Claude-Code-driven extractions on a target codebase. The agent-driven consumption flow amplifies impact: a poisoned `manifest.json` may end up directly in Claude Code's context window. |
| **Malicious contributor** | Submits a PR that looks benign but slips a subtly-malicious pattern, fixture, or dep change. Mitigated by gate 17 (48 h public review window) and gates 11–12 (license + supply-chain audits). |
| **Compromised maintainer credential** | Steals a maintainer's GitHub auth and pushes a malicious release. Mitigated by signed commits (BUILD-PLAN session 18, `gitsign`), Sigstore-signed releases (session 25), branch protection, and crates.io Trusted Publishing OIDC (session 25; no long-lived API tokens). |

**Out of scope for v0.1:**

- Nation-state-grade adversaries with hardware-level supply-chain capability.
- Adversaries with physical access to maintainer hardware.
- Adversaries running malicious code on the operator's host *before*
  `superglue` runs.

## Asset inventory

| Asset | Location | Trust |
|---|---|---|
| Source code | `supergluepro/*` GitHub repos | Trusted; branch-protected; signed commits planned (BUILD-PLAN session 18). |
| Pattern library | `supergluepro/patterns` (planned, Phase E) | Trusted; certification levels planned (session 138). |
| Fixture corpus content | `supergluepro/fixture-corpus` (planned, session 15) | Trusted: pinned by SHA; rotation gated (Phase I). |
| Release artefacts | GitHub Releases + crates.io + Homebrew + Scoop + Winget + AUR + Nix + Docker + apt + rpm + GitHub Action + install.sh / install.ps1 | Trusted: Sigstore-signed (session 25), SLSA L3 attestations (session 24), CycloneDX 1.6 SBOM (session 24). |
| Maintainer credentials | GitHub auth, SSH key, future PGP key | Trusted; not stored in repos; OIDC flows preferred for publishing (session 25). |
| End-user input to `superglue` | Git URL, local path, optionally a fetched repo | **Untrusted** — see Surface 1 below. |
| Output of `superglue` | `SUPERGLUE.md` + `manifest.json` + `confidence-report.json` | Treated as **untrusted by downstream agent** — see Surface 2 below. |

## Surface-by-surface STRIDE-lite

### Surface 1 — CLI consumer (planned, session 14 = `v0.0.1-alpha`)

Invocation: `superglue <git-url-or-path> [--out DIR]`. Inputs are an
untrusted repository (user-supplied URL or path). Output is files written
to `--out`.

| Threat | Mitigation |
|---|---|
| **S — Spoofing** | Git transport: HTTPS or SSH per URL; clone uses `libgit2` / `git2` with default verification. No bypass of TLS or SSH host-key checks. |
| **T — Tampering** | Output written to a user-controlled directory; refuse to overwrite outside `--out`. No symlink escape — paths canonicalised before write. |
| **R — Repudiation** | Each output file carries the input git SHA, walker config, and tool version in its header so consumers can audit. |
| **I — Information disclosure** | Walker stays inside the cloned repo. Default `.gitignore` honoured plus an explicit ignore-list for secrets-likely paths (`.env*`, `*.key`, `id_rsa*`, etc.). Native gitleaks-equivalent scan (planned, session 133) flags secrets in the output. |
| **D — Denial of service** | Walker bounded: max-files, max-bytes-per-file, max-depth, max-runtime. Property + fuzz tests on parsers (planned, sessions 23, 29). Pattern engine ReDoS-resistant by construction (`ast-grep` matchers, not raw regex) — verified per pattern. |
| **E — Elevation of privilege** | CLI runs as the invoking user; no `setuid`; no `sudo` paths; no spawned subprocess that escapes the user's shell. |

### Surface 2 — Output schema as agent input (planned, session 13 freezes the v0 contract)

The output of `superglue` is consumed by Claude Code, Cursor, Aider, etc.
Anything in the output flows into an LLM context window. **This is the
most novel attack surface in the project** — it is not a traditional
file-format threat model; it is an LLM-input threat model.

| Threat | Mitigation |
|---|---|
| **S — Spoofing** | Output files carry: input git SHA, tool version, walker config, output schema version. Consumers can verify provenance. |
| **T — Tampering** | Output is regenerable from input; consumers can re-run `superglue` to verify. Per-pattern provenance metadata (planned, session 137). |
| **I — Information disclosure** | The output may include code snippets, file paths, and pattern matches. Configurable redaction for paths matching the ignore lists. The secrets-scan signal flags secret-bearing files in `confidence-report.json` so consumers can choose to exclude them. |
| **Prompt injection** | A user-supplied repo may contain content (in code comments, READMEs, fixture data) crafted to target the consuming LLM. The output schema clearly separates Superglue-Pro-authored fields from verbatim file content; consumers are documented to treat verbatim file content as untrusted. (To be documented in the MCP integration guide, session 47.) |

### Surface 3 — MCP server (planned, Phase C, sessions 39–48)

`superglue serve` exposes MCP tools over stdio + HTTP transports.

| Threat | Mitigation |
|---|---|
| **S — Spoofing** | Capability negotiation per MCP spec (2025-11-25); HTTP transport requires explicit opt-in + bind address (default `localhost` only); no remote-by-default. |
| **T — Tampering** | Tool schemas frozen and SemVer'd; extends the session-13 v0 output-schema contract without breaking it. |
| **R — Repudiation** | Server emits structured logs; no PII captured. |
| **I — Information disclosure** | Same surface as Surface 2 — the MCP server is just another consumption path for the same output. |
| **D — Denial of service** | Per-tool runtime budgets; rate-limiting on the HTTP transport; bounded watcher queue (`notify-debouncer-full`, session 43). |
| **E — Elevation of privilege** | Server runs as the invoking user; no auth other than transport-level (MCP capability negotiation). |

### Surface 4 — Pattern library (planned, Phase E, sessions 62–101)

YAML pattern packs in `supergluepro/patterns`. Externally contributable.

| Threat | Mitigation |
|---|---|
| **Malicious pattern** | Patterns run as `ast-grep` matchers — no `eval`, no shell-out. Pattern certification levels (session 138): official / community / experimental. Pattern-review committee charter (session 138). Provenance metadata per pattern (session 137). |
| **Pattern that exfiltrates data** | Patterns are pure matchers; they cannot exfiltrate. The output schema is the only side channel; addressed under Surface 2. |
| **Supply-chain pattern injection** | Patterns repo is branch-protected; PRs gated by the per-session 19-gate contract; cargo-deny + cargo-vet + cargo-audit cover Rust deps separately. |

### Surface 5 — Fixture corpus (planned, session 15)

50+ pinned open GitHub repos cloned in CI on every PR.

| Threat | Mitigation |
|---|---|
| **Pinned-fixture upstream compromise** | Each fixture pinned by commit SHA, not branch. CI clones at SHA. Upstream branch force-pushes do not affect us. |
| **Malicious fixture content executed** | CI runs `superglue` against fixture repos; `superglue` itself does not execute fixture content. |
| **Fixture-corpus repo write access compromise** | Same protections as the main repo: branch protection, signed commits, DCO sign-off, 19-gate contract. |

### Surface 6 — Build / release pipeline

| Threat | Mitigation |
|---|---|
| **Compromised maintainer credential** | Sigstore signing (keyless OIDC, session 25); crates.io Trusted Publishing (RFC 3691, OIDC, session 25); long-lived API tokens disabled. |
| **Tampered release artefact** | Sigstore signing + SLSA L3 attestations + CycloneDX 1.6 SBOM (sessions 24+25). Reproducible builds (session 26) so any third party can independently verify. |
| **Compromised CI runner** | OIDC token short-lived (~30 min for crates.io publish per RFC 3691); SHA-pinned actions (session 32); minimum-required `permissions:` per workflow. |
| **Vulnerable dependency** | `cargo audit` + `cargo deny` + `cargo vet` in CI (session 20); Renovate auto-PRs (session 20); RFSI alignment (Phase I). |
| **Typosquatted dep** | RFSI Typomania check applied to publish flows (Phase I). |

### Surface 7 — Distribution paths (planned, Phase D, sessions 49–61)

Eleven+ install paths: `cargo install`, Homebrew, Scoop, Winget, AUR, Nix,
Docker, apt, rpm, GitHub Action, `install.sh` / `install.ps1`.

| Threat | Mitigation |
|---|---|
| **Tampered binary on a third-party mirror** | `cosign verify` documented in README; checksum files alongside every release. |
| **Install script downloads-and-execs from compromised CDN** | `install.sh` / `install.ps1` verify signatures with `cosign verify` before exec; no unsigned binary executed. |
| **Cross-path inconsistency** | Distribution dry-run validation (session 60): every install path against the same fixture set; output mismatch blocks the `v1.0.0` release. |

## Cross-cutting controls already mandated by BUILD-PLAN

These apply to every session, not a specific surface:

- **Per-session 19-gate contract** — every PR; includes gate 11 (license +
  supply-chain green), gate 12 (security checklist), gate 16 (glue-only).
- **DCO sign-off enforcement** — `.github/workflows/dco.yml`, every PR.
- **48 h public review window** — gate 17, surface-affecting changes.
- **Random-open-repo CI gate (gate 19)** — from session 15 onward, every PR.
- **Annual external security audit** — NCC / Trail of Bits / Cure53;
  pre-engagement session 36, completion session 139.

## Out of scope for v0.1

These surfaces don't exist yet and are not modelled in v0.1; they will be
added when they ship:

- WASM playground (browser execution surface, planned session 136).
- LLM-judge model (~150 MB native model, planned session 130).
- Embeddings (`fastembed-rs`, planned session 129).
- Cross-repo federation auth (planned session 132).

## Update cadence

This document is bumped to v0.x.y on every architectural change that adds
or materially modifies a surface. Major version bumps coincide with
BUILD-PLAN phase milestones:

| Version | When | Notes |
|---|---|---|
| v0.1 | BUILD-PLAN session 4 (this revision) | Pre-alpha; most surfaces planned-not-built. |
| v0.2 | End of Phase A (post-`v0.0.1-alpha`) | CLI surface concretised. |
| v0.3 | End of Phase B (post-`v0.1.0`) | Hardening pass folded in (CodeQL, fuzz, SBOM, signing). |
| v1.0 | End of Phase C (post-MCP-native) | MCP server surface added. |
| v1.1 | End of Phase D (post-`v1.0.0`) | All distribution paths concretised. |
| v2.0 | End of Phase H (post-`v2.0.0`) | Embeddings + LLM-judge + WASM + federation surfaces added. |

---

**Last updated:** 2026-04-26 (BUILD-PLAN session 4).
**Document version:** v0.1.
**Reporting a vulnerability:** see [`SECURITY.md`](SECURITY.md).
