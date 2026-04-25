# Superglue Pro — BUILD-PLAN v10

**Version:** v10 — Q2 2026 SOTA refresh + Claude-Code-first consumer + early-runnable + random-open-repo testing rail
**Date:** 2026-04-26
**Supersedes:** v9 (Q2 2026 SOTA verification only) and v8 (this repo, commit `2e7d65a`); the v8 lineage in turn superseded [`supergluepro/superglue` BUILD-PLAN.md v7.1](https://github.com/supergluepro/superglue/blob/main/BUILD-PLAN.md) (closed-source $2/extraction model).

---

## Why this rewrite

v7.1 was sized for a solo founder shipping a closed-source binary gated by a Stripe-backed license server. v8 reframed for OSS. v9 refreshed every tool, standard, and architectural choice against Q2 2026 SOTA. **v10 (this revision)** locks in two further requirements that surfaced after v9:

1. **Claude Code (the CLI) is the canonical consumer.** Superglue Pro is not invoked by humans directly. A developer hints a repo URL to Claude Code; Claude Code clones/fetches and invokes `superglue`; Claude Code consumes the structured output. Every CLI ergonomic, output schema, MCP tool, install path, and doc framing optimises for that flow.
2. **The tool is runnable from Phase A.** A `v0.0.1-alpha` ships at the end of Phase A so the senior dev assigned each downstream session can ask Claude Code to drive Superglue Pro against any GitHub repo and verify the latest implementation works. The plan does not "code in the dark until v0.1.0."
3. **Random-open-repo testing is a hard CI gate from session 15 onward** — not a Phase E nicety. A pinned 50-repo fixture corpus runs on every PR (gate 19); each phase ends with a deep ad-hoc validation session driving Claude Code through never-seen repos to bug-bash before any release tag.

v10 carries forward all v9 SOTA corrections (Contributor Covenant 3.0, MCP/AAIF governance, `trim-paths`, Rust 2024 edition, Vue 3.x pin, Fallow ADR for clone/unused-export rewrites) and additions (`cargo-nextest`, crates.io Trusted Publishing, SBOM via `cargo-cyclonedx`, `cargo-llvm-cov`, `git-cliff`, `cargo-shear`, `typos-cli` + `cargo-spellcheck`, Renovate, WASI 0.3 / Component Model ADR, Rust Foundation Security Initiative alignment).

Three resolved decisions from v8 still hold:

1. **Open source, free for commercial use.** Permissive license, no payment, no license server, no per-install identity. The closed-source codebase in `supergluepro/superglue` is being refactored down to OSS scope ([Phase A](#phase-a--foundation--early-runnability-sessions-117)).
2. **GitHub is the only home.** `supergluepro.com` redirects here. No separate marketing site, no docs subdomain, no hosted offering, no Discord/Slack pre-traction. GitHub Discussions is the community surface; GitHub Pages off the production repo is the docs surface if needed.
3. **Linear sessions, world-expert per session.** A 135-senior-dev talent pool exists; each session pulls the best-credentialed person available for that session's topic, executes one session at a time, and returns the dev to the pool. No parallel squads. Calendar time = sum of session times.

Combined effect: ~50% of v7.1 scope deletes (Phase 4 license server, Phase 5 binary protection, Phase 11 web app, S0 paperwork chain, billing surface). Replaced by OSS ecosystem investments (governance, distribution, MCP-native rearchitecture, pattern library expansion, IDE extensions, advanced features) plus the v10 early-runnable + random-repo testing rail. Total: 141 sessions.

---

## Primary consumer: Claude Code (the CLI)

Superglue Pro's canonical user is **Claude Code** (Anthropic's CLI) and other MCP-capable AI coding agents (Cursor, Aider, Continue, Cline, Codex CLI, Goose). Humans interact with Superglue Pro **indirectly**: a developer says to Claude Code "run superglue on github.com/foo/bar," Claude Code clones/fetches the repo and invokes `superglue`, then consumes the structured output (`SUPERGLUE.md` + `manifest.json` + `confidence-report.json`) or — once Phase C ships — queries the Superglue MCP server for tool-shaped access.

This shapes every part of the plan:

- **CLI ergonomics** (session 13). The invocation pattern is `superglue <git-url-or-path> [--out DIR]`. No interactive prompts. No TUI. No colour-by-default in non-tty. Output is deterministic, machine-readable, self-describing, and bit-identical for the same inputs.
- **Early runnability** (session 14). `v0.0.1-alpha` ships at the end of Phase A. From session 15 onward, the senior dev for any session can ask Claude Code to drive `superglue` against any open GitHub repo. No "wait until v1.0 to test."
- **Random-open-repo testing rail** (session 15 + gate 19 + Phase I rotation + per-phase deep-validation sessions). 50 fixture repos pinned by SHA; CI runs every PR; baseline snapshots; manifest-diff failures block merge. Deep ad-hoc Claude-Code-driven validation in sessions 37 (pre-`v0.1.0`), 48 (post-MCP), 60 (pre-`v1.0.0`), 140 (pre-`v2.0.0`).
- **MCP-native v1.0** (Phase C). The MCP server is the headline consumption surface for Claude Code from `v1.0.0`. Static `SUPERGLUE.md` is the fallback for non-MCP environments. ADR-006 (session 7) freezes "Claude Code as primary consumer" as a project-wide invariant.
- **Documentation framing** (sessions 16, 47). README's first install path is "How to use this from Claude Code." All examples show Claude-Code-driven flow.
- **Output schema is a v0 contract** (session 13). Schema for `manifest.json` and `confidence-report.json` is frozen as a JSON Schema in session 13 and bumped only with major-version SemVer changes. MCP tool schemas (session 39) extend it without breaking it.

The 17-gate per-session contract (next section) plus gate 19 (random-repo CI smoke) plus the four deep-validation sessions are the testing spine of this plan. **No session merges with a red random-repo CI**, and no release tags ship without a clean deep-validation sweep.

## Constraints

| Constraint | Implication |
|---|---|
| Permissive OSS, free for commercial use | No proprietary core. No AGPL/SSPL/BSL. No future-relicense games. |
| GitHub-only public surface | `supergluepro.com` is a redirect. Docs in-repo + GitHub Pages off `supergluepro/superglue` if needed. Marketplace is the patterns repo. |
| Linear sessions | No parallel work. Each session must be self-contained, not break the next session, and not depend on a future session. |
| 12/10 per-session quality | See [per-session contract](#per-session-1210-contract-every-session-honors-all-of-these). World-expert lead, two cross-domain reviewers, mandatory gates. |
| 135-dev talent pool | Sessions can pull genuinely-best-on-Earth expertise per topic. Quality bar moves accordingly. |
| Glue-only philosophy preserved | Inherited from `supergluepro/superglue` CLAUDE.md: top-tier OSS dependencies composed; never reinvent. License compatibility verified before any new dep added. |
| **Claude Code is the canonical consumer** | Humans give a hint (repo URL or path); Claude Code (or any MCP-capable agent) executes the run and consumes the structured output. CLI ergonomics, output schemas, MCP tools, distribution paths, and docs all optimise for this flow. |
| **Tool runnable end-to-end by end of Phase A** | `v0.0.1-alpha` released at session 14. Every downstream session must keep the tool runnable; a session that breaks `superglue` against the fixture corpus does not merge. |
| **Random-open-repo testing is a hard CI gate** | From session 15 onward, every PR's CI runs the pinned fixture corpus (gate 19). Failures attached to the PR block merge unless triaged + intentionally accepted via ADR. |

## Open decisions before session 1

These two questions gate the first sessions and must be answered first.

### 1. Trademark + naming

"Superglue" alone collides with [superglue.com](https://superglue.com) (B2B integration tool). Three options:

- **Register `Superglue Pro`** as the OSS trademark; matches org name. Globally registrable.
- **Rename pre-launch** to a distinct mark (`ctxr`, `tier`, `repocontext`, `archlens`, others).
- **Stay generic** — no trademark, rely on `supergluepro` org as the anchor.

Working assumption throughout this plan: **register `Superglue Pro`**. Confirm or override before session 1.

### 2. License

- **`MIT OR Apache-2.0` dual** — Rust ecosystem standard. Matches existing `Cargo.toml` workspace declaration. Downstream consumers pick. **Recommended.**
- **Apache-2.0 alone** — clearer patent grant, matches Kubernetes / Envoy / Tekton convention.

Both are permissive and "free for commercial use." Decide before session 1.

---

## Per-session 12/10 contract (every session honors all of these)

| # | Gate | Specifics |
|---:|---|---|
| 1 | Expert match | Session lead is the best-credentialed person available for the session's topic. No "good enough" matches. |
| 2 | Two cross-domain reviewers | At least one reviewer from a different specialty than the lead. Catches blind spots. |
| 3 | RFC if architectural | Public RFC merged before code starts; no "we'll write it up later." |
| 4 | ADR for internal decisions | Every "why this and not that" recorded in `/adr/`. |
| 5 | Determinism-first tests | Tests written before feature; runner is `cargo-nextest` (process-per-test isolation). Working Rule 6 from the existing CLAUDE.md preserved across all 141 sessions. |
| 6 | Mutation score ≥80% | `cargo-mutants` on touched modules (running under `cargo-nextest`). Public mutation-score badge updated. |
| 7 | Property-based tests dominant | `proptest` for any parsing / serialisation / idempotency surface. |
| 8 | Performance budget | Bench-before / bench-after; ≥5% regression blocks merge. |
| 9 | Reproducibility | Built artefact reproduces bit-identically. |
| 10 | `cargo-semver-checks` green | No accidental SemVer breakage outside major-version bumps. |
| 11 | License + supply-chain green | `cargo deny`, `cargo audit`, `cargo vet` all green. |
| 12 | Security checklist | Threat-model touch points reviewed; SECURITY.md updated if disclosure surface changes. |
| 13 | CHANGELOG.md updated | Keep-a-Changelog format, semver-tagged. |
| 14 | Snapshot tests refreshed | `insta` updates committed; no orphan `.snap.new` files. |
| 15 | CI matrix green | Linux x86_64+aarch64 (glibc+musl), macOS Intel+ARM, Windows; stable + MSRV. |
| 16 | Glue-only honored | Zero reinvented libraries; license compatibility verified for any new dep added. |
| 17 | Public review window | 48h public review on PR before merge for surface-affecting changes. |
| 18 | SBOM published | Each tagged release ships a CycloneDX 1.6 SBOM (via `cargo-cyclonedx`) alongside SLSA L3 provenance. |
| 19 | **Random-open-repo smoke gate** | From session 15 onward, every PR's CI runs `superglue` against the full fixture corpus (50 pinned open GitHub repos). Manifest-diffs against baselines surfaced as PR comments; any unexpected failure blocks merge. New stack/grammar sessions (Phase E/F) add ≥3 fixtures for that stack/grammar. |

The bar is non-negotiable. A session that cannot meet all 19 gates is not merged; the session is split or descoped until each piece meets the bar.

## Cross-session machinery (built once, used always)

These are the rails the linear sessions run on. Every session inherits and updates them.

- **RFC repo** (`supergluepro/rfcs`) — Rust-language-team-style template, ≥10-day public comment window, two reviewer approvals
- **ADR directory** (`/adr/0001-…md`) — Michael Nygard format
- **Style guide** (`/docs/style.md`) — Rust API guidelines + project conventions
- **CHANGELOG.md** — Keep a Changelog 1.1.0 format, generated by `git-cliff` from Conventional Commits
- **CONTRIBUTING.md** — DCO sign-off required (lighter than CLA, sufficient for permissive OSS)
- **CODE_OF_CONDUCT.md** — Contributor Covenant 3.0
- **SECURITY.md** — 90-day responsible disclosure, PGP key, threat-model location, RFSI alignment
- **GOVERNANCE.md** — single-maintainer + Technical Steering Committee path documented
- **Session runbook** (`/docs/sessions/SESSION-TEMPLATE.md`) — what every session lead reads before starting
- **PR template + Code-review template** — gates 1–19 mechanised as checklists
- **Dependency update bot** — Renovate config in repo root (`renovate.json`); auto-PR for cargo, GitHub Actions, Docker; auto-merge for patch-level after green CI
- **Random-repo fixture corpus** (`supergluepro/fixture-corpus`) — 50 open GitHub repos pinned by commit SHA, baseline snapshot per repo, CI workflow that runs `superglue` against every fixture on every PR; rotation rail in Phase I

These are the deliverables of sessions 1–8 (governance) plus sessions 13–15 (CLI surface, alpha, fixture corpus). After that they're maintained, not rebuilt.

---

## Phase A — Foundation + early runnability (sessions 1–17)

OSS conversion + cross-session machinery + Claude-Code-first CLI surface + first runnable alpha + random-repo testing rail + first public release-candidate.

| # | Session | Notes |
|---:|---|---|
| 1 | License pick + LICENSE-MIT + LICENSE-APACHE + NOTICE files | Legal expert (Heather Meeker / Allison Randal-tier). Working assumption: `MIT OR Apache-2.0` dual. |
| 2 | CONTRIBUTING.md with DCO + Signed-off-by enforcement | Linux-kernel-style sign-off; gate at PR check via `dcoapp/app` (Probot DCO) or GitHub Action. |
| 3 | CODE_OF_CONDUCT.md (Contributor Covenant 3.0) | Released 2026; reorganises into "Encouraged" / "Restricted" behaviours and a revised enforcement ladder. v2.1 is superseded (Django adopted v3 on 2026-04-15). 30-min session. |
| 4 | SECURITY.md + threat-model v0.1 | NCC / Cure53 / Trail of Bits-tier security expert. PGP key generation; 90-day disclosure window; CVE coordination process; explicit alignment with Rust Foundation Security Initiative outputs (Typomania, crate-quarantine RFC, provenance tracking). |
| 5 | GOVERNANCE.md + steering structure | Single-maintainer with documented TSC promotion path (≥3 maintainers triggers TSC formation). |
| 6 | RFC repo setup + first RFC template | Rust-language-team alum lead. Repo: `supergluepro/rfcs`. |
| 7 | ADR format + first 6 ADRs documenting current architecture | Michael Nygard format. ADRs cover: tier-based confidence (existing); ast-grep over hand-rolled matcher; Postgres+Valkey for license server (now archived); MCP as v1.0 architecture; OSS pivot rationale; **ADR-006 — Claude Code as primary consumer** (CLI invocation, output-schema-as-contract, MCP-native v1.0 alignment). |
| 8 | Style guide doc + PR / code-review templates | Rust API guidelines + project conventions; checklist-mechanised quality gates 1–19. |
| 9 | Tag `pre-oss-pivot` on `supergluepro/superglue` `main`; delete `crates/server/` | ~13K LOC removed. License-server end-of-life. |
| 10 | Delete CLI billing surface | `cli/src/{auth,reserve,feedback,device_flow,install_id,net}.rs` + tests. ~5K LOC. |
| 11 | Refactor `cli/src/extract.rs` to strip server-trip code | `ServerConfig`, `reserve_credit`, `post_confirm`, `prompt_and_post_feedback`, S35b helpers all removed. ~700 LOC. |
| 12 | `cargo machete` + `cargo shear` + `cargo deny` audit; remove unused workspace deps | Run both `cargo-machete` (regex, fast) and `cargo-shear` (rust-analyzer AST, catches macro-only / build-script usage). Strip `async-stripe`, `oauth2`, `argon2`, `jsonwebtoken`, `axum`, `tower`, `tower-http`, `sqlx`, `redis`, `sentry`, `tracing`, `tracing-subscriber`, `windows-sys`, `qrcode`, `mockito`, `testcontainers`. |
| 13 | **Claude-Code-first CLI surface design** (RFC + ADR-006 expansion) | CLI/UX expert (jq / ripgrep / Stripe-CLI-tier). Freeze: invocation `superglue <git-url-or-path> [--out DIR] [--manifest-only] [--quiet]`; output formats `SUPERGLUE.md` + `manifest.json` + `confidence-report.json` as JSON-Schema-validated v0 contracts; exit-code conventions; streaming-friendly stdout for Claude Code consumption; MCP tool stub schemas drafted (full impl in Phase C). No interactive prompts ever. |
| 14 | **First runnable alpha — `v0.0.1-alpha`** | Tag `v0.0.1-alpha` after sessions 9–13. `cargo install --git https://github.com/supergluepro/superglue` works on Linux/macOS/Windows. README has a "30-second install for Claude Code" snippet. First manual smoke test: ask Claude Code to run `superglue` against three hand-picked open repos (a Next.js app, a Django service, a Go HTTP server) and verify the manifest is well-formed and Claude Code can consume it. Findings logged. |
| 15 | **Random-open-repo fixture corpus + CI smoke gate (gate 19 goes live)** | Fixture-engineering / corpus-curation expert. Curate 50 open GitHub repos pinned by commit SHA, balanced across grammars (TS/JS/Python/Go/Rust/Java/Ruby/PHP/etc.) and frameworks (Next/Django/Express/Gin/Spring/Rails/Laravel/etc.). Repo: `supergluepro/fixture-corpus`. Each fixture: `repo.toml` (URL + SHA + expected-grammar list), baseline `SUPERGLUE.md` + `manifest.json` snapshots committed. CI workflow: clones each fixture at pinned SHA, runs `superglue`, diffs against baseline; manifest-diff failures attached as PR comments. From this session onward, gate 19 is mandatory. |
| 16 | README rewrite as product page — Claude-Code-first messaging | Pieter-Levels-tier copy / Stripe-Press-tier writer. One-sentence pitch frames Claude Code as the canonical user: *"You point Claude Code at a repo. Claude Code runs Superglue Pro. Claude Code gets the right files, in the right order, with the right confidence scores."* Three-platform install on screen one. Asciinema cast shows Claude-Code-driven flow, not human-driven. |
| 17 | `supergluepro.com` → GitHub redirect | Single-page static at `supergluepro/supergluepro.github.io` with apex CNAME. `<meta http-equiv="refresh">` + JS fallback. 5-min session. |

**Milestone:** end of Phase A — codebase is OSS-clean, governance complete, Claude-Code-first CLI surface frozen as v0 contract, **`v0.0.1-alpha` runnable and installable**, random-repo fixture corpus live with CI smoke gate enforced. Public surface (GitHub + redirect) live. From here, every downstream session inherits gate 19 and a working tool against which Claude Code can verify behaviour.

---

## Phase B — Quality hardening (sessions 18–38)

The infrastructure that earns enterprise trust on day one. Every session lands on top of a runnable tool; gate 19 is enforced from the first PR.

| # | Session | Notes |
|---:|---|---|
| 18 | Branch protection + signed commits | Sigstore `gitsign` (keyless OIDC) for maintainers; required for `main`. |
| 19 | CI matrix: Linux/macOS/Windows × stable/MSRV/beta; `cargo-nextest` as test runner; Rust 2024 edition workspace-wide | GitHub Actions; cache `~/.cargo`; `cargo-nextest` is the de-facto Rust test runner in 2026 (process-per-test isolation, ~3× faster than `cargo test`, native RustRover/IntelliJ support). Workspace `edition = "2024"` declared in root `Cargo.toml`. |
| 20 | `cargo-audit` + `cargo-deny` + `cargo-vet` + `typos-cli` + `cargo-spellcheck` in CI; Renovate enabled | Vouched dep chain; private mirror for build determinism. `typos-cli` for source spelling; `cargo-spellcheck` for doc-comments; Renovate auto-PRs Cargo / GitHub Actions / Docker bumps with auto-merge for patch-level after green CI. |
| 21 | CodeQL workflow for Rust | GitHub-native SAST (GA since CodeQL 2.23.3, October 2025); nightly + on-PR. |
| 22 | `cargo-mutants` in CI | Mutation score ≥80% on critical crates. Runs under `cargo-nextest`. Public badge. |
| 23 | `cargo-fuzz` setup; 6 fuzz targets | Pattern loader, YAML parser, ast-grep input, walker, completeness, output renderer. |
| 24 | SLSA L3 attestations + CycloneDX 1.6 SBOM | SLSA L3 via `slsa-framework/slsa-github-generator`, verifiable with `slsa-verifier`. SBOM via `cargo-cyclonedx` (also emits SPDX 2.2.1 if needed for ISO compliance). Both attached to every GitHub Release. |
| 25 | Sigstore-signed releases + crates.io Trusted Publishing (OIDC) | `cosign sign-blob` (Cosign v3, GA) on every release artefact; `cosign verify` documented in README. crates.io Trusted Publishing per RFC 3691 — `rust-lang/crates-io-auth-action` exchanges the GitHub Actions OIDC token for a 30-min publish token; long-lived API tokens disabled and Trusted-only enforced for all crates. |
| 26 | Reproducible builds | `trim-paths` (default in release builds since RFC 3127), `SOURCE_DATE_EPOCH`, `--locked` deps, `CARGO_INCREMENTAL=0`, `RUSTFLAGS="-C debuginfo=0"`; quarterly third-party reproduction verification. |
| 27 | `cargo-semver-checks` in CI | Catches SemVer violations on PR. |
| 28 | MSRV policy doc + MSRV-aware resolver + CI gate | MSRV = stable − 6 months (sliding window); MSRV-aware resolver (stable since Rust 1.84, implicit since 2024 edition / 1.85) declared in `Cargo.toml` `package.rust-version`. Deprecation 6-month minimum window. `cargo-msrv` verifies in CI. |
| 29 | Property-test expansion | `proptest` across walker, hash, completeness, aggregate. Hundreds of properties, not "weight-sum=1.0" alone. |
| 30 | Benchmark suite (`criterion`) + perf-budget CI gate; `cargo-llvm-cov` coverage gate | Per-signal runtime budgets; ≥5% regression blocks merge. Coverage via `cargo-llvm-cov` (LLVM source-based instrumentation; integrates with `cargo-nextest`); coverage-floor CI gate; LCOV uploaded to public dashboard for badge. |
| 31 | OpenSSF Best Practices badge application | Gold-tier path. |
| 32 | OpenSSF Scorecard hardening to 9.5+ | Public Scorecard badge. |
| 33 | Public mutation-score / coverage / Scorecard badges in README | Surfaces the quality investment to visitors. |
| 34 | Pre-built binaries via `cargo-dist` | Linux x86_64+aarch64 (glibc+musl), macOS Intel+ARM, Windows. Latest `cargo-dist` 0.31.0 (Feb 2026). |
| 35 | Universal `install.sh` + `install.ps1` | Checksum + signature verification (`cosign verify`). |
| 36 | Pre-engagement of annual external security audit | NCC / Trail of Bits / Cure53 RFP. Public report committed. |
| 37 | **Deep random-repo validation pre-`v0.1.0`** | Full fixture-corpus run + 50 ad-hoc Claude-Code-driven runs against never-seen open GitHub repos picked by reviewers (not in the fixture corpus). Bug bash. Each finding becomes a tracked issue; severity ≥P1 blocks the v0.1.0 tag. Outputs become regression fixtures in the corpus. |
| 38 | **`v0.1.0` release — public Show HN** | First public artefact. README + asciinema cast + install commands tested across all three platforms. Claude-Code-driven invocation is the headline demo. |

**Milestone:** Phase B end. Public OSS launch. Public OpenSSF Scorecard, mutation-score, coverage badges. SLSA L3 + Sigstore signed binaries. Audit pre-engaged. Tool validated against the full fixture corpus and 50 hand-picked never-seen repos via Claude Code.

---

## Phase C — MCP-native rearchitecture (sessions 39–48)

The headline architectural differentiator vs. Repomix / gitingest / Aider repo-map. Every step is validated by having Claude Code consume the MCP server live.

| # | Session | Notes |
|---:|---|---|
| 39 | RFC: MCP server architecture | MCP core maintainer (Agentic AI Foundation / Linux Foundation, donated December 2025) or Anthropic MTS-tier contributor lead. Tool schemas, capability negotiation, error model. Aligned with current MCP spec (2025-11-25) and AAIF Working Group cadence. |
| 40 | RFC: SUPERGLUE.md → MCP migration semantics | Back-compat preserved; static `SUPERGLUE.md` remains the fallback for non-MCP environments. Session 13's v0 output-schema contract is the floor; MCP tools extend without breaking it. |
| 41 | `superglue serve` subcommand skeleton | Stdio + HTTP transports; capability advertisement. |
| 42 | MCP tools — first batch | `tier1_files`, `find_pattern`, `signal_report`, `ai_context_for(file)`, `diff_since(commit)`. |
| 43 | `superglue watch` mode | `notify` crate (+ `notify-debouncer-full`) file watcher; MCP push-on-change semantics. |
| 44 | Agent simulation harness | Claude Code, Cursor, Aider, Cline, Codex CLI, Goose testers; CI integration. |
| 45 | Pattern-version pinning over MCP | Agents can request "patterns validated against Next.js 15.x." |
| 46 | MCP integration tests against real agents | Live tests in CI sandbox. |
| 47 | Documentation: MCP integration guide — Claude-Code-first | `/docs/mcp.md` in production repo. First example is "How Claude Code consumes our MCP server." |
| 48 | **Random-repo MCP integration deep test** | Real Claude Code (latest stable) consumes our MCP server against 20 random open GitHub repos. Validates capability negotiation, tool-schema correctness, watch-mode push semantics, error pathing under real consumption. Findings folded back into MCP tools batch (session 42) before merge. Outputs become regression fixtures. |

**Milestone:** Superglue is the first MCP-native code-context tool with first-party agent integration tests, validated end-to-end through live Claude Code consumption.

---

## Phase D — Distribution (sessions 49–61)

One package manager per session. Validation step at the end ensures every install path produces a working binary that Claude Code can drive.

| # | Session | Notes |
|---:|---|---|
| 49 | Homebrew core PR | Separate from any tap; full review process. |
| 50 | Scoop bucket | Windows OSS-default. |
| 51 | Winget package | Microsoft Store equivalent. |
| 52 | AUR package | Arch Linux. |
| 53 | Nix flake | `nix run github:supergluepro/superglue` works out of the box. |
| 54 | Docker image | GHCR + Docker Hub, multi-arch, signed (cosign), SLSA-attested. |
| 55 | GitHub Action composite | Marketplace listing. "Extract on PR comment" reference workflow with Claude-Code-driven invocation. |
| 56 | apt repo via GitHub Releases | Signed `.deb` artefacts in releases; no separate apt server. |
| 57 | rpm repo via GitHub Releases | Signed `.rpm` artefacts. |
| 58 | `release-plz` + `git-cliff` automation; release-train cadence | `release-plz` drives versioning + crates.io publish (with `cargo-semver-checks` gate); `git-cliff` generates Conventional-Commit changelogs (Keep-a-Changelog 1.1.0 format). Release train every 2 weeks. |
| 59 | Release notes pipeline | Auto-generated from CHANGELOG + manually curated highlights. |
| 60 | **Distribution dry-run + random-repo install validation** | Spin up clean VMs/containers per platform, install via every package manager (Homebrew, Scoop, Winget, AUR, Nix, Docker, apt, rpm, `cargo install`, `install.sh`/`install.ps1`); for each install path, ask Claude Code to run `superglue` against 20 random open GitHub repos. Verify identical output across paths. Mismatches block `v1.0.0`. |
| 61 | **`v1.0.0` release — feature-complete OSS** | All seven signals + MCP server + watch mode + pattern library v1 + 11+ install paths, validated through Claude Code on every path. |

**Milestone:** end of Phase D — Superglue is installable on every developer's preferred toolchain and verified working under Claude-Code-driven consumption from each path. SemVer 1.0; backward-compatibility commitments begin.

---

## Phase E — Pattern library expansion (sessions 62–101)

The moat. 75 → ~2000 patterns. Per session: one stack expert authors a complete pattern pack (~50 patterns) for one stack with full fixture coverage, AI_CONTEXT templates, integration tests, **and ≥3 random-open-repo fixtures of that stack added to the corpus** (gate 19 enforces).

| # | Stack |
|---:|---|
| 62 | Django (Python) |
| 63 | Rails (Ruby) |
| 64 | Laravel (PHP) |
| 65 | SvelteKit |
| 66 | Vue 3.x + Vite |
| 67 | Astro |
| 68 | Remix |
| 69 | Express (Node) |
| 70 | Koa (Node) |
| 71 | Fastify (Node) |
| 72 | Spring Boot (Java) |
| 73 | ASP.NET Core (C#) |
| 74 | FastAPI (Python) |
| 75 | Flask (Python) |
| 76 | Phoenix (Elixir) |
| 77 | Gin (Go) |
| 78 | Echo (Go) |
| 79 | Fiber (Go) |
| 80 | Axum (Rust) |
| 81 | Rocket (Rust) |
| 82 | Actix (Rust) |
| 83 | htmx |
| 84 | Solid |
| 85 | Qwik |
| 86 | Nuxt |
| 87 | Hono |
| 88 | Cloudflare Workers |
| 89 | AWS Lambda (Node + Python + Rust) |
| 90 | GCP Cloud Functions |
| 91 | Azure Functions |
| 92 | Ktor (Kotlin) |
| 93 | Quarkus (Java) |
| 94 | Bun runtime |
| 95 | Deno |
| 96 | Tauri 2 |
| 97 | Electron |
| 98 | React Native |
| 99 | Flutter |
| 100 | Vapor (Swift) |
| 101 | Yew (Rust front-end) |

**Milestone:** end of Phase E — pattern library at ~2000 patterns across 40 stacks. Pattern marketplace de-facto established (the patterns directory itself). Fixture corpus has grown to ≥170 repos (50 baseline + ≥3 per stack × 40 stacks).

---

## Phase F — Tree-sitter grammar expansion (sessions 102–121)

5 → 25 grammars. Per session: one grammar expert, full integration + pattern coverage + fixture corpus + **≥3 random-open-repo fixtures using that grammar added to the corpus**.

| # | Grammar |
|---:|---|
| 102 | rust |
| 103 | java |
| 104 | kotlin |
| 105 | swift |
| 106 | ruby |
| 107 | php |
| 108 | c |
| 109 | cpp |
| 110 | csharp |
| 111 | scala |
| 112 | elixir |
| 113 | clojure |
| 114 | ocaml |
| 115 | haskell |
| 116 | lua |
| 117 | dart |
| 118 | julia |
| 119 | R |
| 120 | terraform |
| 121 | dockerfile |

**Milestone:** Superglue covers the long tail of language ecosystems. Pattern packs for Rust / Java / Kotlin / Swift / Ruby become possible. Fixture corpus at ≥230 repos.

---

## Phase G — IDE extensions (sessions 122–128)

One IDE per session, including marketplace publication. Each extension's primary use-case is "trigger Superglue Pro the way Claude Code does, but from inside the editor."

| # | Session | Marketplace |
|---:|---|---|
| 122 | VS Code extension | VS Code Marketplace + Open VSX (AWS-backed since Mar 2026; required for Cursor / Windsurf / Antigravity / Kiro forks) |
| 123 | JetBrains plugin | JetBrains Marketplace (signed) |
| 124 | Neovim Lua plugin | `:Plug 'supergluepro/superglue.nvim'` |
| 125 | Emacs package | MELPA |
| 126 | Sublime Text plugin | Package Control |
| 127 | Helix integration | `~/.config/helix/languages.toml` (no plugin system; this is config-level integration) |
| 128 | Zed integration | Zed Extensions registry (license required since Oct 2025) |

**Milestone:** Superglue is one keystroke away in every major IDE.

---

## Phase H — Advanced features (sessions 129–141)

The "killer features" — done properly, not as flags. All validated end-to-end through Claude Code before the v2.0 tag.

| # | Session | Notes |
|---:|---|---|
| 129 | Embeddings: `fastembed-rs` integration | Semantic pattern search; default-on; ~80MB native model. ONNX-based, well-respected, MIT/Apache. |
| 130 | LLM-judge: tier-confidence validation | Small native model (~150MB); ships as `superglue-cli-with-judge` install variant. |
| 131 | Pattern-mining mode (`superglue mine`) | AST n-gram extraction; pattern proposal; PR-ready YAML output. Multi-session research stream may break this into 2–3 sub-sessions. |
| 132 | Cross-repo federation MVP | `superglue federate org/*`; unified knowledge graph across an org's repos; MCP-exposed; primary consumer remains Claude Code. |
| 133 | WASM-clean signal rewrites — gitleaks → native | Pure-Rust secret-pattern engine; removes subprocess dependency for WASM target. (Note: Betterleaks (2026, by gitleaks creator) is pure Go without CGO — drop-in for gitleaks CLI but not WASM-portable for our use; native Rust rewrite still required.) |
| 134 | WASM-clean signal rewrites — knip → native | **ADR-required prerequisite:** evaluate [Fallow](https://github.com/fallow-rs/fallow) (Rust-native, 90 framework plugins, sub-second on Next.js, ships `fallow migrate` from knip). Either depend on / fork Fallow ("glue-only" preserved) or document why a fresh rewrite is necessary. Pure-Rust unused-export detector either way. |
| 135 | WASM-clean signal rewrites — jscpd → native | **ADR-required prerequisite:** evaluate Fallow's clone-detection layer (same Rust-native upstream as session 134) and `cargo-dupes` (AST-normalised fingerprints + Dice coefficient for near-duplicates). Either depend on / fork or document why a fresh rewrite is necessary. Pure-Rust clone detector either way. |
| 136 | Full WebAssembly target | **ADR-required prerequisite:** WASI 0.3 / WebAssembly Component Model (WIT IDL, async-native, GA Feb 2026) vs. classic `wasm32-unknown-unknown` browser-only target. Browser playground hosted on `supergluepro.github.io/playground` (`wasm-bindgen` v0.2.118 + `wasm-pack` v0.14.0). All seven signals work in browser; component-model artefact additionally usable from Wasmtime / non-browser runtimes if ADR selects it. |
| 137 | Provenance-aware patterns | Per-pattern metadata: framework version, author, last-validated-against. |
| 138 | RFC: pattern certification levels | Official / community / experimental. Pattern review committee charter. |
| 139 | First annual third-party security audit completion | NCC / Trail of Bits / Cure53 report published in repo. CVEs (if any) disclosed and patched. |
| 140 | **Deep random-repo validation pre-`v2.0.0`** | Full fixture corpus run (≥230 repos) + 100 ad-hoc Claude-Code-driven runs against never-seen open GitHub repos. Validation specifically of: embeddings semantic search, LLM-judge tier-confidence, cross-repo federation, WASM playground (browser + Wasmtime). Each finding becomes a tracked issue; severity ≥P1 blocks the v2.0 tag. Outputs become permanent regression fixtures. |
| 141 | **`v2.0.0` release — embeddings + LLM-judge + WASM playground default** | Headline release: "Superglue Pro is now a complete code-context platform for AI coding agents." |

**Milestone:** end of Phase H — Superglue Pro is the reference-class AI-context tool. Industry-default status pursued. Tool validated against ≥230 fixture repos and ≥170 hand-picked never-seen repos via Claude Code over the project lifetime.

---

## Phase I — Ongoing rails (continuous)

Not numbered sessions; ambient discipline that runs from Phase A onward.

- Annual third-party security audit cycle (NCC / Trail of Bits / Cure53)
- OSS-Fuzz application + integration once accepted (acceptance not guaranteed; cargo-fuzz is the baseline)
- Quarterly dependency-graph audit
- **Random-open-repo fixture corpus rotation** — every 5 merged sessions: add 1 newly-curated open repo to the corpus, retire 1 stale fixture (low-signal or upstream-deleted). Public transparency log of failures + their fixes attached to the corpus repo.
- **Claude-Code-driven ad-hoc validation cadence** — at minimum every 10 merged sessions, the maintainer (or a session lead) asks the latest stable Claude Code to run `superglue` against 5 random not-yet-seen open repos. Findings logged; regressions become fixtures.
- Rust Foundation Security Initiative alignment — track RFSI outputs (Typomania typosquatting detection, crate-quarantine RFC, provenance-tracking work); apply to `supergluepro/*` crates as they ship.
- Monthly community office hours (recorded; transcript posted to GitHub Discussions)
- RFC review cadence (≥10-day public window per RFC; biweekly batched merges)
- Conference talk preparation (KubeCon, RustConf, AI Engineer Summit, GitHub Universe — sponsored booths and talks)
- Showcase ramp: collect corporate-user testimonials with permission
- CNCF Sandbox application packet (target: end of Phase E; contingent on demonstrated multi-org production usage; OpenSSF Best Practices badge already required and earned in session 31)

---

## Calendar

| Phase | Sessions | Calendar (1 session ≈ 2–3 days expert-led) |
|---|---:|---|
| A — Foundation + early runnability | 17 | 5–7 weeks |
| B — Quality hardening | 21 | 7–11 weeks |
| C — MCP rearchitecture | 10 | 3–5 weeks |
| D — Distribution | 13 | 4–7 weeks |
| E — Pattern library | 40 | 13–20 weeks |
| F — Grammars | 20 | 6–10 weeks |
| G — IDE extensions | 7 | 2–4 weeks |
| H — Advanced features | 13 | 4–7 weeks |
| **Total** | **141 sessions** | **~14–22 months** |

**Public release tags:**
- `v0.0.1-alpha` at end of Phase A (~5–7 weeks in) — first runnable; Claude Code can drive it
- `v0.1.0` at end of Phase B (~3–5 months in) — OSS public Show HN, hardened, signed, SBOM, validated
- `v1.0.0` at end of Phase D (~6–8 months in) — feature-complete, all install paths, MCP-native, validated through every install path
- `v2.0.0` at end of Phase H (~14–22 months in) — embeddings + LLM-judge + WASM playground + cross-repo federation

---

## What lives where

| Repo | Visibility | Purpose |
|---|---|---|
| `supergluepro/supergluepro` (this repo) | Public | OSS planning home: build plan, governance scaffolding, ADRs, RFCs index. The "meta" repo. |
| `supergluepro/superglue` | Currently private; flips public end of Phase A | Production code, patterns, AI_CONTEXT templates, grammar integrations. The Rust workspace. |
| `supergluepro/rfcs` | Public (created in session 6) | RFC discussions for architectural changes. Rust-language-team-style. |
| `supergluepro/patterns` | Public (created in Phase E) | Pattern library separated from main code repo for easier external PRs. |
| `supergluepro/fixture-corpus` | Public (created in session 15) | Random-open-repo fixture corpus: pinned SHAs, baseline `SUPERGLUE.md` + `manifest.json` snapshots, CI workflow, transparency log. The testing spine of the project. |
| `supergluepro/supergluepro.github.io` | Public | Single-page static redirect from `supergluepro.com` to the GitHub org; later hosts WASM playground at `/playground` (session 136). |

---

## Sequencing rules

1. **No session starts** until prior session is merged + CI green.
2. **No session merges** without all 19 quality gates honored.
3. **No public surface change** without a 48h public review window on the PR.
4. **No new dependency added** without `cargo deny` + `cargo audit` + `cargo vet` clean and license verified.
5. **No SemVer break** outside major version bumps (`v0.x` → `v1.0`, `v1.x` → `v2.0`).
6. **Every architectural decision is recorded** as an ADR before code changes; every public-surface decision as an RFC before merge.
7. **No release tag** (`v0.1.0`, `v1.0.0`, `v2.0.0`) without a clean deep-validation sweep (sessions 37, 60, 140 respectively) — including never-seen-repo runs driven by Claude Code.
8. **Output-schema contract** (frozen in session 13) cannot break in any session below a major-version bump. MCP tool schemas (Phase C) extend without breaking it.

This plan is itself a living document. Amendments require an RFC against this repo. Session reordering requires no RFC if dependencies are preserved; session content changes require RFC if they affect public surface or quality gates.

---

## Out of scope (explicit non-goals — drop if proposed)

- Cross-repo federation as a separate company (one feature in Phase H is enough; not a Glean-equivalent)
- Plugin system in `v1.0` — patterns + MCP cover extensibility
- GUI / desktop app (terminal + IDE extensions are sufficient; Tauri is dropped from the plan)
- AGPL with future-relicense option (kills enterprise adoption immediately)
- Discord pre-traction (GitHub Discussions only until ≥1k stars)
- Mobile app
- Browser extension (defer; WASM playground covers the browser surface)
- Press kit / brand guidelines pre-launch
- Tutorial videos pre-launch (DevRel content production scales with traction)
- CNCF Sandbox application before demonstrating multi-org production usage
- Multi-language docs i18n (defer; English-only until traction justifies)
- Hosted offering / commercial layer (explicitly excluded by constraint #2)
- **Human-first CLI ergonomics (TUI, interactive prompts, colour-by-default in pipes)** — explicitly contrary to the Claude-Code-first constraint. If a proposed feature is justified by "this is nicer for a human typing at a terminal," it is rejected. Humans hint; Claude Code runs.

These items are tracked here so future PRs proposing them get a documented rationale for rejection.
