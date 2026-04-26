# Session log

Chronological log of completed sessions in BUILD-PLAN.md. Each entry captures
deliverables, gate compliance, decisions, and deferred follow-ups so the next
session lead (or fresh Claude Code instance) can pick up cold. Newest entry on
top. Once `/docs/sessions/SESSION-TEMPLATE.md` lands in session 8, future
entries will follow that template and may move into a `/sessions/` directory;
this single-file format is the bootstrap convention until then.

---

## Session 2 — CONTRIBUTING.md + DCO enforcement (2026-04-26)

**Lead:** solo bootstrap (Claude Opus 4.7 1M-context + Dipl.-Ing.(FH) Meysam Shiehzadeh)
**BUILD-PLAN row:** session 2 — "CONTRIBUTING.md with DCO + Signed-off-by enforcement"

### Decisions resolved

1. **Inbound contributor attestation: DCO via `git commit -s`, not a CLA.**
   Linux-kernel-style sign-off attests the contributor's right to submit; the
   Apache-2.0-§5-style clause in `README.md` is the actual licensing
   mechanism. A CLA was rejected as heavier than necessary for a
   permissive-OSS project (separate document, signing process, contributor
   record-keeping). Will be back-filled as **ADR-003** in session 7.
2. **Enforcement mechanism: in-repo GitHub Actions workflow, not the Probot
   DCO App** (`dcoapp/app`). Keeps the gate self-contained in the
   repository, avoids a third-party app dependency, aligns with the
   "GitHub-only public surface" constraint. The workflow walks every
   non-merge commit between PR base and head, parses trailers via
   `git interpret-trailers --parse`, and emits per-commit
   `::error title=Missing DCO sign-off::…` annotations on miss. Will be
   back-filled as **ADR-004** in session 7.
3. **Conventional Commits required up-front.** `git-cliff` regenerates
   `CHANGELOG.md` from Conventional-Commit messages from session 58 onward;
   documenting the requirement now avoids a retroactive history rewrite.
   The `session-N:` prefix from session 1 is preserved as additionally
   acceptable for governance/planning sessions.
4. **Pre-session-2 commits are not retroactively signed off.** Commits
   `2e7d65a`, `68c92df`, `2e3c30a`, `d22291f` (initial commit + BUILD-PLAN
   evolution + session 1) lack `Signed-off-by:` trailers. Rewriting them
   would change SHAs already on `origin/main` and is not worth the
   disruption for a meta-repo with one contributor at this stage. The DCO
   workflow only inspects PR commits (`base..head`), so historical commits
   on `main` do not retro-fail. Going forward, every commit (including
   direct-to-`main` bootstrap commits) carries the trailer.

### Files

| Action | Path | Note |
|---|---|---|
| add | `CONTRIBUTING.md` | DCO sign-off process, Conventional Commits, PR/issue flow, pointers to CoC (session 3) / RFC repo (session 6) / ADR directory (session 7). |
| add | `.github/workflows/dco.yml` | Hand-rolled DCO check; only third-party action used is `actions/checkout@v4`. SHA-pinning deferred to session 32 (Scorecard hardening); Renovate (session 20) will manage digests. |
| modify | `README.md` | + top-level `## Contributing` section pointing at `CONTRIBUTING.md` (the existing `### Contribution` legal clause under `## License` stays as-is). |
| modify | `CHANGELOG.md` | + Added: `CONTRIBUTING.md`, `dco.yml`. + Decided: DCO over CLA, in-repo workflow over Probot. + Changed: README contributing section. |
| modify | `SESSION-LOG.md` | This entry. |

### Per-session gate sweep

| # | Gate | Status |
|---:|---|---|
| 11 | License + supply-chain green | ✅ No new dependencies introduced. |
| 13 | CHANGELOG.md updated | ✅ Added / Decided / Changed entries land alongside files. |
| 16 | Glue-only honored | ✅ DCO workflow is hand-rolled inline (no third-party DCO Action / Probot dependency); only `actions/checkout` (official, GitHub-published) is consumed. |
| 1, 2, 3, 4, 17 | Expert / reviewers / RFC / ADR / 48 h public review | ⚠️ Symbolic during bootstrap, same as session 1. RFC repo not built yet (session 6); ADR format not built yet (session 7). DCO + workflow decisions to be back-filled as ADR-003 / ADR-004 in session 7. |
| 5–10, 12, 14–15, 18–19 | Test/perf/repro/SemVer/security/SBOM/random-repo | N/A — no Rust code, no CI matrix, no release, no fixture corpus at this stage. The DCO workflow itself will run live on the first PR after `supergluepro/supergluepro` is on GitHub. |

### Deferred follow-ups

- **Session 3 (next):** `CODE_OF_CONDUCT.md` (Contributor Covenant 3.0,
  released 2026, reorganised into "Encouraged" / "Restricted" with a revised
  enforcement ladder; v2.1 superseded). Once it lands, `CONTRIBUTING.md`'s
  "Code of Conduct" pointer becomes a live link.
- **Session 4:** `SECURITY.md` will publish a PGP key + dedicated disclosure
  channel; until then, security reports go to `meysam@shiehzadeh.de`.
  Replace the stand-in pointer in `CONTRIBUTING.md` § Communication.
- **Session 6:** RFC repo `supergluepro/rfcs` lands. Update the RFC pointer
  in `CONTRIBUTING.md` from "once that repo is created in session 6" to a
  live link to the RFC template + recent RFCs.
- **Session 7:** ADR directory `/adr/` lands. Back-fill **ADR-001**
  (license = `MIT OR Apache-2.0` dual), **ADR-002** (brand = unregistered,
  anchored by domain + org), **ADR-003** (DCO over CLA), **ADR-004**
  (in-repo Action over Probot DCO). ADRs 005 (MCP as v1.0 architecture)
  and 006 (Claude Code as primary consumer) are forward-looking and land
  with their respective sessions.
- **Session 8:** PR + code-review templates mechanise gates 1–19 as
  checklists; they should reference `CONTRIBUTING.md` § DCO and the
  workflow file directly.
- **Session 20:** Enable Renovate; ensure its config covers GitHub Actions
  (so `actions/checkout` gets digest-pinned auto-PRs).
- **Session 32:** OpenSSF Scorecard hardening to ≥9.5. The
  "Pinned-Dependencies" check requires SHA-pinned actions. Replace
  `actions/checkout@v4` with `actions/checkout@<sha> # v4.x.x` then.
- **End of Phase A (when `supergluepro/superglue` flips public):** replicate
  `CONTRIBUTING.md` (extended with Rust toolchain / build / test
  instructions) and `.github/workflows/dco.yml` in that repo. The
  meta-repo's CONTRIBUTING already announces that the production repo will
  carry its own variant covering the code-side workflow.

### State for the next session

Tracked tree at the end of session 2:

```
.github/
  workflows/
    dco.yml          — DCO Signed-off-by enforcement on every PR
.gitignore           — standard + .claude/settings.local.json + user-text.txt excluded
BUILD-PLAN.md        — v10, "Resolved decisions" section
CHANGELOG.md         — Keep-a-Changelog 1.1.0; sessions 1+2 entries under [Unreleased]
CONTRIBUTING.md      — DCO + Conventional Commits + RFC/ADR/CoC pointers
LICENSE-APACHE       — Apache 2.0 full text
LICENSE-MIT          — MIT
NOTICE               — minimal Apache-style notice
README.md            — License + new ## Contributing pointer; no Open decisions
SESSION-LOG.md       — sessions 1+2
```

Untracked / gitignored: `.claude/settings.local.json` and `user-text.txt`.

Session 3 (`CODE_OF_CONDUCT.md`, Contributor Covenant 3.0) is a ~30-min
session per the BUILD-PLAN row. The text is downloaded verbatim from the
Covenant project and committed; no project-specific tailoring needed beyond
filling the contact address (`meysam@shiehzadeh.de` until session 4 ships
the dedicated security/conduct disclosure channels).

---

## Session 1 — License pick (2026-04-26)

**Lead:** solo bootstrap (Claude Opus 4.7 1M-context + Dipl.-Ing.(FH) Meysam Shiehzadeh)
**BUILD-PLAN row:** session 1 — "License pick + LICENSE-MIT + LICENSE-APACHE + NOTICE files"

### Decisions resolved

1. **License: `MIT OR Apache-2.0` dual.** Rust-ecosystem standard; downstream
   consumers pick at use time. Inbound contributor licensing
   dual-licensed-to-match per README's Apache-2.0-§5-style clause.
2. **Trademark / brand: stay generic — no formal registration.** Brand anchored by:
   - `supergluepro.com` — held by Dipl.-Ing.(FH) Meysam Shiehzadeh, registered
     2026-04-15 via united-domains GmbH (DE), expires 2027-04-15. Currently on
     united-domains parking nameservers (`ns.udag.de` / `ns.udag.net` /
     `ns.udag.org`).
   - `supergluepro` GitHub org.
   - Consistent use of "Superglue Pro" as the project display name.

Both decisions retire the prior v10 working assumptions in BUILD-PLAN/README's
"Open decisions before session 1" section, which is now "Resolved decisions"
(BUILD-PLAN) / removed (README, since the answers are reflected throughout).

### Files

| Action | Path | Note |
|---|---|---|
| add | `LICENSE-MIT` | Standard MIT text. © 2026 Dipl.-Ing.(FH) Meysam Shiehzadeh. |
| add | `LICENSE-APACHE` | Apache 2.0 full text; copyright notice in the appendix. |
| add | `NOTICE` | Project name + dual-license election. |
| add | `CHANGELOG.md` | Keep-a-Changelog 1.1.0 stub; regenerated by `git-cliff` from session 58. |
| add | `SESSION-LOG.md` | This file. |
| modify | `README.md` | + License section (dual-license footer, SPDX `MIT OR Apache-2.0`); − "Open decisions before session 1". |
| modify | `BUILD-PLAN.md` | "Open decisions before session 1" → "Resolved decisions" with rationale + operational follow-ups. |
| modify | `.gitignore` | Exclude `.claude/settings.local.json` and `user-text.txt`. |

### Per-session gate sweep

| # | Gate | Status |
|---:|---|---|
| 11 | License + supply-chain green | ✅ The license is the deliverable; no dependencies yet to audit. |
| 13 | CHANGELOG.md updated | ✅ Stub created; Unreleased section captures Added / Decided / Changed. |
| 16 | Glue-only honored | ✅ No new dependencies introduced. |
| 1, 2, 3, 4, 17 | Expert / reviewers / RFC / ADR / 48 h public review | ⚠️ Symbolic during bootstrap. RFC repo not built yet (session 6); ADR format not built yet (session 7). License + brand decisions to be back-filled as ADR-001 / ADR-002 in session 7. 48 h public review symbolic on a meta-repo with one contributor. |
| 5–10, 14–15, 18–19 | Test/perf/repro/SemVer/security/SBOM/random-repo | N/A — no code, no CI, no release, no fixture corpus at this stage. |

### Deferred follow-ups

- **Session 2 (next):** `CONTRIBUTING.md` with DCO + Signed-off-by enforcement
  via `dcoapp/app` (Probot DCO) or a GitHub Action. Linux-kernel-style sign-off.
- **Session 7:** Back-fill `ADR-001` (license = `MIT OR Apache-2.0` dual) and
  `ADR-002` (brand = unregistered, anchored by domain + org).
- **Session 17:** Replace united-domains parking nameservers on
  `supergluepro.com` with the GitHub redirect (single-page static at
  `supergluepro/supergluepro.github.io`).
- **Calendar reminder (operational, not session-scoped):** renew
  `supergluepro.com` before 2027-04-15, or switch to multi-year /
  auto-renew in the united-domains panel. Candidate for `/schedule` background
  agent ~2027-02 to verify renewal status.

### State for the next session

Tracked tree at the end of session 1:

```
.gitignore           — standard + .claude/settings.local.json + user-text.txt excluded
BUILD-PLAN.md        — v10, "Resolved decisions" section
CHANGELOG.md         — Keep-a-Changelog 1.1.0 stub, Unreleased
LICENSE-APACHE       — Apache 2.0 full text
LICENSE-MIT          — MIT
NOTICE               — minimal Apache-style notice
README.md            — License section live; no Open decisions section
SESSION-LOG.md       — this log
```

Untracked / gitignored: `.claude/settings.local.json` (Claude Code user-local)
and `user-text.txt` (user's personal session-runner playbook). The production
code repo `supergluepro/superglue` remains private and is not pulled locally
this session — Phase A sessions 1–8 + 16–17 land governance here in the
meta-repo; sessions 9–15 will operate against `supergluepro/superglue` once it
is accessible.
