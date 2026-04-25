# Superglue Pro — BUILD-PLAN v8

**Version:** v8 — OSS pivot, linear sessions, 12/10 per-session quality bar
**Date:** 2026-04-25
**Supersedes:** [`supergluepro/superglue` BUILD-PLAN.md v7.1](https://github.com/supergluepro/superglue/blob/main/BUILD-PLAN.md) (closed-source $2/extraction model)

---

## Why this rewrite

v7.1 was sized for a solo founder shipping a closed-source binary gated by a Stripe-backed license server. v8 reflects three resolved decisions:

1. **Open source, free for commercial use.** Permissive license, no payment, no license server, no per-install identity. The closed-source codebase in `supergluepro/superglue` is being refactored down to OSS scope ([Phase A](#phase-a--foundation-sessions-114)).
2. **GitHub is the only home.** `supergluepro.com` redirects here. No separate marketing site, no docs subdomain, no hosted offering, no Discord/Slack pre-traction. GitHub Discussions is the community surface; GitHub Pages off the production repo is the docs surface if needed.
3. **Linear sessions, world-expert per session.** A 135-senior-dev talent pool exists; each session pulls the best-credentialed person available for that session's topic, executes one session at a time, and returns the dev to the pool. No parallel squads. Calendar time = sum of session times.

Combined effect: ~50% of v7.1 scope deletes (Phase 4 license server, Phase 5 binary protection, Phase 11 web app, S0 paperwork chain, billing surface). Replaced by OSS ecosystem investments (governance, distribution, MCP-native rearchitecture, pattern library expansion, IDE extensions, advanced features) totalling 134 sessions.

## Constraints

| Constraint | Implication |
|---|---|
| Permissive OSS, free for commercial use | No proprietary core. No AGPL/SSPL/BSL. No future-relicense games. |
| GitHub-only public surface | `supergluepro.com` is a redirect. Docs in-repo + GitHub Pages off `supergluepro/superglue` if needed. Marketplace is the patterns repo. |
| Linear sessions | No parallel work. Each session must be self-contained, not break the next session, and not depend on a future session. |
| 12/10 per-session quality | See [per-session contract](#per-session-1210-contract-every-session-honors-all-of-these). World-expert lead, two cross-domain reviewers, mandatory gates. |
| 135-dev talent pool | Sessions can pull genuinely-best-on-Earth expertise per topic. Quality bar moves accordingly. |
| Glue-only philosophy preserved | Inherited from `supergluepro/superglue` CLAUDE.md: top-tier OSS dependencies composed; never reinvent. License compatibility verified before any new dep added. |

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
| 5 | Determinism-first tests | Tests written before feature. Working Rule 6 from the existing CLAUDE.md preserved across all 134 sessions. |
| 6 | Mutation score ≥80% | `cargo-mutants` on touched modules. Public mutation-score badge updated. |
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

The bar is non-negotiable. A session that cannot meet all 17 gates is not merged; the session is split or descoped until each piece meets the bar.

## Cross-session machinery (built once, used always)

These are the rails the linear sessions run on. Every session inherits and updates them.

- **RFC repo** (`supergluepro/rfcs`) — Rust-language-team-style template, ≥10-day public comment window, two reviewer approvals
- **ADR directory** (`/adr/0001-…md`) — Michael Nygard format
- **Style guide** (`/docs/style.md`) — Rust API guidelines + project conventions
- **CHANGELOG.md** — Keep a Changelog
- **CONTRIBUTING.md** — DCO sign-off required (lighter than CLA, sufficient for permissive OSS)
- **CODE_OF_CONDUCT.md** — Contributor Covenant 2.1
- **SECURITY.md** — 90-day responsible disclosure, PGP key, threat-model location
- **GOVERNANCE.md** — single-maintainer + Technical Steering Committee path documented
- **Session runbook** (`/docs/sessions/SESSION-TEMPLATE.md`) — what every session lead reads before starting
- **PR template + Code-review template** — gates 1–17 mechanised as checklists

These are the deliverables of sessions 1–8. After that they're maintained, not rebuilt.

---

## Phase A — Foundation (sessions 1–14)

OSS conversion + cross-session machinery + first public release.

| # | Session | Notes |
|---:|---|---|
| 1 | License pick + LICENSE-MIT + LICENSE-APACHE + NOTICE files | Legal expert (Heather Meeker / Allison Randal-tier). Working assumption: `MIT OR Apache-2.0` dual. |
| 2 | CONTRIBUTING.md with DCO + Signed-off-by enforcement | Linux-kernel-style sign-off; gate at PR check via `dco-bot` or GitHub Action. |
| 3 | CODE_OF_CONDUCT.md (Contributor Covenant 2.1) | Boilerplate; 30-min session. |
| 4 | SECURITY.md + threat-model v0.1 | NCC / Cure53 / Trail of Bits-tier security expert. PGP key generation; 90-day disclosure window; CVE coordination process. |
| 5 | GOVERNANCE.md + steering structure | Single-maintainer with documented TSC promotion path (≥3 maintainers triggers TSC formation). |
| 6 | RFC repo setup + first RFC template | Rust-language-team alum lead. Repo: `supergluepro/rfcs`. |
| 7 | ADR format + first 5 ADRs documenting current architecture | Michael Nygard format. ADRs cover: tier-based confidence (existing); ast-grep over hand-rolled matcher; Postgres+Valkey for license server (now archived); MCP as v1.0 architecture; OSS pivot rationale. |
| 8 | Style guide doc + PR / code-review templates | Rust API guidelines + project conventions; checklist-mechanised quality gates. |
| 9 | Tag `pre-oss-pivot` on `supergluepro/superglue` `main`; delete `crates/server/` | ~13K LOC removed. License-server end-of-life. |
| 10 | Delete CLI billing surface | `cli/src/{auth,reserve,feedback,device_flow,install_id,net}.rs` + tests. ~5K LOC. |
| 11 | Refactor `cli/src/extract.rs` to strip server-trip code | `ServerConfig`, `reserve_credit`, `post_confirm`, `prompt_and_post_feedback`, S35b helpers all removed. ~700 LOC. |
| 12 | `cargo machete` + `cargo deny` audit; remove unused workspace deps | Strip `async-stripe`, `oauth2`, `argon2`, `jsonwebtoken`, `axum`, `tower`, `tower-http`, `sqlx`, `redis`, `sentry`, `tracing`, `tracing-subscriber`, `windows-sys`, `qrcode`, `mockito`, `testcontainers`. |
| 13 | README rewrite as product page | Pieter-Levels-tier copy / Stripe-Press-tier writer. One-sentence pitch, asciinema cast, three-platform install on screen one. |
| 14 | `supergluepro.com` → GitHub redirect | Single-page static at `supergluepro/supergluepro.github.io` with apex CNAME. `<meta http-equiv="refresh">` + JS fallback. 5-min session. |

**Milestone:** end of Phase A — codebase is OSS-clean, governance complete, public surface (GitHub + redirect) live. No public release yet.

---

## Phase B — Quality hardening (sessions 15–34)

The infrastructure that earns enterprise trust on day one.

| # | Session | Notes |
|---:|---|---|
| 15 | Branch protection + signed commits | Sigstore `gitsign` for maintainers; required for `main`. |
| 16 | CI matrix: Linux/macOS/Windows × stable/MSRV/beta | GitHub Actions; cache `~/.cargo`. |
| 17 | `cargo-audit` + `cargo-deny` + `cargo-vet` in CI | Vouched dep chain; private mirror for build determinism. |
| 18 | CodeQL workflow for Rust | GitHub-native SAST; nightly + on-PR. |
| 19 | `cargo-mutants` in CI | Mutation score ≥80% on critical crates. Public badge. |
| 20 | `cargo-fuzz` setup; 6 fuzz targets | Pattern loader, YAML parser, ast-grep input, walker, completeness, output renderer. |
| 21 | SLSA L3 attestations | Via `slsa-framework/slsa-github-generator`. Verifiable with `slsa-verifier`. |
| 22 | Sigstore-signed releases | `cosign sign-blob` on every release artefact; `cosign verify` documented in README. |
| 23 | Reproducible builds | `SOURCE_DATE_EPOCH`, `--remap-path-prefix`, `--locked` deps; quarterly third-party reproduction verification. |
| 24 | `cargo-semver-checks` in CI | Catches SemVer violations on PR. |
| 25 | MSRV policy doc + CI gate | MSRV stable; deprecation 6-month minimum window. |
| 26 | Property-test expansion | `proptest` across walker, hash, completeness, aggregate. Hundreds of properties, not "weight-sum=1.0" alone. |
| 27 | Benchmark suite (`criterion`) + perf-budget CI gate | Per-signal runtime budgets; ≥5% regression blocks merge. |
| 28 | OpenSSF Best Practices badge application | Gold-tier path. |
| 29 | OpenSSF Scorecard hardening to 9.5+ | Public Scorecard badge. |
| 30 | Public mutation-score / coverage / Scorecard badges in README | Surfaces the quality investment to visitors. |
| 31 | Pre-built binaries via `cargo-dist` | Linux x86_64+aarch64 (glibc+musl), macOS Intel+ARM, Windows. |
| 32 | Universal `install.sh` + `install.ps1` | Checksum + signature verification. |
| 33 | Pre-engagement of annual external security audit | NCC / Trail of Bits / Cure53 RFP. Public report committed. |
| 34 | **`v0.1.0` release — public Show HN** | First public artefact. README + asciinema cast + install commands tested across all three platforms. |

**Milestone:** Phase B end. Public OSS launch. Public OpenSSF Scorecard, mutation-score, coverage badges. SLSA L3 + Sigstore signed binaries. Audit pre-engaged.

---

## Phase C — MCP-native rearchitecture (sessions 35–43)

The headline architectural differentiator vs. Repomix / gitingest / Aider repo-map.

| # | Session | Notes |
|---:|---|---|
| 35 | RFC: MCP server architecture | Anthropic MCP-WG member lead. Tool schemas, capability negotiation, error model. |
| 36 | RFC: SUPERGLUE.md → MCP migration semantics | Back-compat preserved; static `SUPERGLUE.md` remains the fallback for non-MCP environments. |
| 37 | `superglue serve` subcommand skeleton | Stdio + HTTP transports; capability advertisement. |
| 38 | MCP tools — first batch | `tier1_files`, `find_pattern`, `signal_report`, `ai_context_for(file)`, `diff_since(commit)`. |
| 39 | `superglue watch` mode | `notify`-crate file watcher; MCP push-on-change semantics. |
| 40 | Agent simulation harness | Claude Code, Cursor, Aider testers; CI integration. |
| 41 | Pattern-version pinning over MCP | Agents can request "patterns validated against Next.js 15.x." |
| 42 | MCP integration tests against real agents | Live tests in CI sandbox. |
| 43 | Documentation: MCP integration guide | `/docs/mcp.md` in production repo. |

**Milestone:** Superglue is the first MCP-native code-context tool with first-party agent integration tests.

---

## Phase D — Distribution (sessions 44–55)

One package manager per session.

| # | Session | Notes |
|---:|---|---|
| 44 | Homebrew core PR | Separate from any tap; full review process. |
| 45 | Scoop bucket | Windows OSS-default. |
| 46 | Winget package | Microsoft Store equivalent. |
| 47 | AUR package | Arch Linux. |
| 48 | Nix flake | `nix run github:supergluepro/superglue` works out of the box. |
| 49 | Docker image | GHCR + Docker Hub, multi-arch, signed (cosign), SLSA-attested. |
| 50 | GitHub Action composite | Marketplace listing. "Extract on PR comment" reference workflow. |
| 51 | apt repo via GitHub Releases | Signed `.deb` artefacts in releases; no separate apt server. |
| 52 | rpm repo via GitHub Releases | Signed `.rpm` artefacts. |
| 53 | `release-plz` automation + release-train cadence | Every 2 weeks. |
| 54 | Release notes pipeline | Auto-generated from CHANGELOG + manually curated highlights. |
| 55 | **`v1.0.0` release — feature-complete OSS** | All seven signals + MCP server + watch mode + pattern library v1 + 11+ install paths. |

**Milestone:** end of Phase D — Superglue is installable on every developer's preferred toolchain. SemVer 1.0; backward-compatibility commitments begin.

---

## Phase E — Pattern library expansion (sessions 56–95)

The moat. 75 → ~2000 patterns. Per session: one stack expert authors a complete pattern pack (~50 patterns) for one stack with full fixture coverage, AI_CONTEXT templates, and integration tests.

| # | Stack |
|---:|---|
| 56 | Django (Python) |
| 57 | Rails (Ruby) |
| 58 | Laravel (PHP) |
| 59 | SvelteKit |
| 60 | Vue 3 + Vite |
| 61 | Astro |
| 62 | Remix |
| 63 | Express (Node) |
| 64 | Koa (Node) |
| 65 | Fastify (Node) |
| 66 | Spring Boot (Java) |
| 67 | ASP.NET Core (C#) |
| 68 | FastAPI (Python) |
| 69 | Flask (Python) |
| 70 | Phoenix (Elixir) |
| 71 | Gin (Go) |
| 72 | Echo (Go) |
| 73 | Fiber (Go) |
| 74 | Axum (Rust) |
| 75 | Rocket (Rust) |
| 76 | Actix (Rust) |
| 77 | htmx |
| 78 | Solid |
| 79 | Qwik |
| 80 | Nuxt |
| 81 | Hono |
| 82 | Cloudflare Workers |
| 83 | AWS Lambda (Node + Python + Rust) |
| 84 | GCP Cloud Functions |
| 85 | Azure Functions |
| 86 | Ktor (Kotlin) |
| 87 | Quarkus (Java) |
| 88 | Bun runtime |
| 89 | Deno |
| 90 | Tauri 2 |
| 91 | Electron |
| 92 | React Native |
| 93 | Flutter |
| 94 | Vapor (Swift) |
| 95 | Yew (Rust front-end) |

**Milestone:** end of Phase E — pattern library at ~2000 patterns across 40 stacks. Pattern marketplace de-facto established (the patterns directory itself).

---

## Phase F — Tree-sitter grammar expansion (sessions 96–115)

5 → 25 grammars. Per session: one grammar expert, full integration + pattern coverage + fixture corpus.

| # | Grammar |
|---:|---|
| 96 | rust |
| 97 | java |
| 98 | kotlin |
| 99 | swift |
| 100 | ruby |
| 101 | php |
| 102 | c |
| 103 | cpp |
| 104 | csharp |
| 105 | scala |
| 106 | elixir |
| 107 | clojure |
| 108 | ocaml |
| 109 | haskell |
| 110 | lua |
| 111 | dart |
| 112 | julia |
| 113 | R |
| 114 | terraform |
| 115 | dockerfile |

**Milestone:** Superglue covers the long tail of language ecosystems. Pattern packs for Rust / Java / Kotlin / Swift / Ruby become possible.

---

## Phase G — IDE extensions (sessions 116–122)

One IDE per session, including marketplace publication.

| # | Session | Marketplace |
|---:|---|---|
| 116 | VS Code extension | VS Code Marketplace + Open VSX |
| 117 | JetBrains plugin | JetBrains Marketplace |
| 118 | Neovim Lua plugin | `:Plug 'supergluepro/superglue.nvim'` |
| 119 | Emacs package | MELPA |
| 120 | Sublime Text plugin | Package Control |
| 121 | Helix integration | `~/.config/helix/languages.toml` |
| 122 | Zed integration | Zed Extensions registry |

**Milestone:** Superglue is one keystroke away in every major IDE.

---

## Phase H — Advanced features (sessions 123–134)

The "killer features" — done properly, not as flags.

| # | Session | Notes |
|---:|---|---|
| 123 | Embeddings: `fastembed-rs` integration | Semantic pattern search; default-on; ~80MB native model. |
| 124 | LLM-judge: tier-confidence validation | Small native model (~150MB); ships as `superglue-cli-with-judge` install variant. |
| 125 | Pattern-mining mode (`superglue mine`) | AST n-gram extraction; pattern proposal; PR-ready YAML output. Multi-session research stream may break this into 2–3 sub-sessions. |
| 126 | Cross-repo federation MVP | `superglue federate org/*`; unified knowledge graph across an org's repos; MCP-exposed. |
| 127 | WASM-clean signal rewrites — gitleaks → native | Pure-Rust secret-pattern engine; removes subprocess dependency for WASM target. |
| 128 | WASM-clean signal rewrites — knip → native | Pure-Rust unused-export detector. |
| 129 | WASM-clean signal rewrites — jscpd → native | Pure-Rust clone detector. |
| 130 | Full WebAssembly target | Browser playground hosted on `supergluepro.github.io/playground`. All seven signals work in browser. |
| 131 | Provenance-aware patterns | Per-pattern metadata: framework version, author, last-validated-against. |
| 132 | RFC: pattern certification levels | Official / community / experimental. Pattern review committee charter. |
| 133 | First annual third-party security audit completion | NCC / Trail of Bits / Cure53 report published in repo. CVEs (if any) disclosed and patched. |
| 134 | **`v2.0.0` release — embeddings + LLM-judge + WASM playground default** | Headline release: "Superglue Pro is now a complete code-context platform." |

**Milestone:** end of Phase H — Superglue Pro is the reference-class AI-context tool. Industry-default status pursued.

---

## Phase I — Ongoing rails (continuous)

Not numbered sessions; ambient discipline that runs from Phase A onward.

- Annual third-party security audit cycle (NCC / Trail of Bits / Cure53)
- OSS-Fuzz application + integration once accepted (acceptance not guaranteed; cargo-fuzz is the baseline)
- Quarterly dependency-graph audit
- Monthly community office hours (recorded; transcript posted to GitHub Discussions)
- RFC review cadence (≥10-day public window per RFC; biweekly batched merges)
- Conference talk preparation (KubeCon, RustConf, AI Engineer Summit, GitHub Universe — sponsored booths and talks)
- Showcase ramp: collect corporate-user testimonials with permission
- CNCF Sandbox application packet (target: end of Phase E; contingent on demonstrated multi-org production usage)

---

## Calendar

| Phase | Sessions | Calendar (1 session ≈ 2–3 days expert-led) |
|---|---:|---|
| A — Foundation | 14 | 4–6 weeks |
| B — Quality hardening | 20 | 6–10 weeks |
| C — MCP rearchitecture | 9 | 3–4 weeks |
| D — Distribution | 12 | 4–6 weeks |
| E — Pattern library | 40 | 13–20 weeks |
| F — Grammars | 20 | 6–10 weeks |
| G — IDE extensions | 7 | 2–4 weeks |
| H — Advanced features | 12 | 4–6 weeks |
| **Total** | **134 sessions** | **~14–20 months** |

**Public release tags:**
- `v0.1.0` at end of Phase B (~3–4 months in) — OSS launch
- `v1.0.0` at end of Phase D (~5–6 months in) — feature-complete, all install paths
- `v2.0.0` at end of Phase H (~14–20 months in) — embeddings + LLM-judge + WASM playground

---

## What lives where

| Repo | Visibility | Purpose |
|---|---|---|
| `supergluepro/supergluepro` (this repo) | Public | OSS planning home: build plan, governance scaffolding, ADRs, RFCs index. The "meta" repo. |
| `supergluepro/superglue` | Currently private; flips public end of Phase A | Production code, patterns, AI_CONTEXT templates, grammar integrations. The Rust workspace. |
| `supergluepro/rfcs` | Public (created in session 6) | RFC discussions for architectural changes. Rust-language-team-style. |
| `supergluepro/patterns` | Public (created in Phase E) | Pattern library separated from main code repo for easier external PRs. |
| `supergluepro/supergluepro.github.io` | Public | Single-page static redirect from `supergluepro.com` to the GitHub org. |

---

## Sequencing rules

1. **No session starts** until prior session is merged + CI green.
2. **No session merges** without all 17 quality gates honored.
3. **No public surface change** without a 48h public review window on the PR.
4. **No new dependency added** without `cargo deny` + `cargo audit` + `cargo vet` clean and license verified.
5. **No SemVer break** outside major version bumps (`v0.x` → `v1.0`, `v1.x` → `v2.0`).
6. **Every architectural decision is recorded** as an ADR before code changes; every public-surface decision as an RFC before merge.

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

These items are tracked here so future PRs proposing them get a documented rationale for rejection.
