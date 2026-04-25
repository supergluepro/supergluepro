# Superglue Pro

> You hint Claude Code at a repo. Claude Code runs Superglue Pro. Claude Code gets the right files, in the right order, with the right confidence scores.

**Status: in planning.** The product is in active development; this repo holds the OSS roadmap, governance scaffolding, and the session-by-session build plan. Code lives in [`supergluepro/superglue`](https://github.com/supergluepro/superglue) (currently private; transitioning to OSS via [Phase A](BUILD-PLAN.md#phase-a--foundation--early-runnability-sessions-117) of the build plan).

## What this is

A CLI that walks a codebase, runs seven signals (git churn, comment density, complexity, test existence, dead-code, duplication, pattern-match) in parallel, scores every file 0–100, sorts into four tiers, and emits a structured context bundle (`SUPERGLUE.md` + `manifest.json` + `confidence-report.json`) that AI coding agents can consume.

## Who it's for

**Claude Code (the CLI) is the canonical user.** Humans hint a repo URL to Claude Code; Claude Code clones/fetches and invokes `superglue`; Claude Code consumes the output or — once Phase C ships — queries the Superglue MCP server for tool-shaped access. Cursor, Aider, Continue, Cline, Codex CLI, and Goose are first-class consumers via the same MCP surface. CLI ergonomics, output schemas, install paths, and docs are all optimised for that flow. Static `SUPERGLUE.md` is the fallback for non-MCP environments.

## What lives in this repo

- [`BUILD-PLAN.md`](BUILD-PLAN.md) — the 141-session sequence: foundation + early runnable alpha (Phase A), quality hardening (B), MCP rearchitecture (C), distribution (D), pattern library (E), grammar expansion (F), IDE extensions (G), advanced features (H)
- Governance scaffolding (added across [Phase A](BUILD-PLAN.md#phase-a--foundation--early-runnability-sessions-117) sessions): LICENSE, CONTRIBUTING, CODE_OF_CONDUCT (Contributor Covenant 3.0), SECURITY, GOVERNANCE
- Architectural Decision Records (`/adr/`, populated from session 7 onward; ADR-006 freezes "Claude Code as primary consumer")
- RFC repository pointer (lives at `supergluepro/rfcs`, populated from session 6 onward)
- Fixture-corpus pointer (lives at `supergluepro/fixture-corpus`, created in session 15) — 50+ pinned open GitHub repos that CI runs `superglue` against on every PR

Production code, patterns, AI_CONTEXT templates, and grammar integrations live in `supergluepro/superglue`.

## Public release tags

- `v0.0.1-alpha` — end of Phase A (~5–7 weeks in). First runnable; `cargo install --git github.com/supergluepro/superglue` works; Claude Code can drive it. **From here, every downstream session lands on a working tool.**
- `v0.1.0` — end of Phase B (~3–5 months in). OSS public Show HN; SLSA L3 + Sigstore-signed; SBOM (CycloneDX 1.6); validated by deep Claude-Code-driven random-repo bug-bash.
- `v1.0.0` — end of Phase D (~6–8 months in). Feature-complete, MCP-native, 11+ install paths, validated through every install path.
- `v2.0.0` — end of Phase H (~14–22 months in). Embeddings + LLM-judge + WASM playground + cross-repo federation.

## Open decisions before session 1

Two questions gate the first sessions:

1. **Trademark + naming.** "Superglue" alone collides with [superglue.com](https://superglue.com) (a B2B integration tool). Working name throughout this plan is "Superglue Pro" matching the org name. Decision required: register `Superglue Pro` as the OSS trademark, rename pre-launch, or stay generic.
2. **License.** Working assumption is `MIT OR Apache-2.0` dual (Rust ecosystem standard, matches existing `Cargo.toml` declaration). Apache-2.0 alone is the alternative. Decision required before session 1.

## Constraints

- **Single GitHub home.** All public surface is on GitHub. `supergluepro.com` redirects here; no separate marketing site, docs site, or hosted offering.
- **Linear development.** One session at a time. No parallel squads. Each session pulls the best-credentialed expert from a 135-person talent pool.
- **12/10 per-session quality bar — 19 gates.** See [the per-session contract](BUILD-PLAN.md#per-session-1210-contract-every-session-honors-all-of-these) in the build plan. Includes mutation score ≥80%, SLSA L3 + SBOM per release, and a hard random-open-repo CI smoke gate from session 15 onward.
- **Permissive OSS, free for commercial use.** No proprietary core, no AGPL/SSPL/BSL, no dual-licensing games.
- **Claude Code is the canonical consumer.** Humans hint; Claude Code runs. CLI ergonomics, output schemas, MCP tools, and docs all optimise for that flow.
- **Tool runnable from end of Phase A.** `v0.0.1-alpha` ships at session 14; the senior dev for any later session can ask Claude Code to verify the latest implementation against any open GitHub repo.
