# Superglue Pro

> Extract the most important files from a codebase for AI context.

**Status: in planning.** The product is in active development; this repo holds the OSS roadmap, governance scaffolding, and the session-by-session build plan. Code lives in [`supergluepro/superglue`](https://github.com/supergluepro/superglue) (currently private; transitioning to OSS via [Phase A](BUILD-PLAN.md#phase-a--foundation-sessions-114) of the build plan).

## What this is

A CLI that walks a codebase, runs seven signals (git churn, comment density, complexity, test existence, dead-code, duplication, pattern-match) in parallel, scores every file 0–100, sorts into four tiers, and emits a structured context bundle (`SUPERGLUE.md` + `manifest.json` + `confidence-report.json`) that AI coding agents can consume.

Designed to be MCP-native: agents (Claude Code, Cursor, Aider, Continue, Cline, Codex CLI) connect via Model Context Protocol and query the repo through structured tools rather than reading static files. Static `SUPERGLUE.md` is the fallback for non-MCP environments.

## What lives in this repo

- [`BUILD-PLAN.md`](BUILD-PLAN.md) — the 134-session sequence to v2.0
- Governance scaffolding (added across [Phase A](BUILD-PLAN.md#phase-a--foundation-sessions-114) sessions): LICENSE, CONTRIBUTING, CODE_OF_CONDUCT, SECURITY, GOVERNANCE
- Architectural Decision Records (`/adr/`, populated from session 7 onward)
- RFC repository pointer (lives at `supergluepro/rfcs`, populated from session 6 onward)

Code, patterns, AI_CONTEXT templates, and grammar integrations live in the production repo `supergluepro/superglue`.

## Open decisions before session 1

Two questions gate the first sessions:

1. **Trademark + naming.** "Superglue" alone collides with [superglue.com](https://superglue.com) (a B2B integration tool). Working name throughout this plan is "Superglue Pro" matching the org name. Decision required: register `Superglue Pro` as the OSS trademark, rename pre-launch, or stay generic.
2. **License.** Working assumption is `MIT OR Apache-2.0` dual (Rust ecosystem standard, matches existing `Cargo.toml` declaration). Apache-2.0 alone is the alternative. Decision required before session 1.

## Constraints

- **Single GitHub home.** All public surface is on GitHub. `supergluepro.com` redirects here; no separate marketing site, docs site, or hosted offering.
- **Linear development.** One session at a time. No parallel squads. Each session pulls the best-credentialed expert from a 135-person talent pool.
- **12/10 per-session quality bar.** See [the per-session contract](BUILD-PLAN.md#per-session-1210-contract-every-session-honors-all-of-these) in the build plan.
- **Permissive OSS, free for commercial use.** No proprietary core, no AGPL/SSPL/BSL, no dual-licensing games.
