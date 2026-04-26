# Session log

Chronological log of completed sessions in BUILD-PLAN.md. Each entry captures
deliverables, gate compliance, decisions, and deferred follow-ups so the next
session lead (or fresh Claude Code instance) can pick up cold. Newest entry on
top. Once `/docs/sessions/SESSION-TEMPLATE.md` lands in session 8, future
entries will follow that template and may move into a `/sessions/` directory;
this single-file format is the bootstrap convention until then.

---

## Session 6 — RFC repo setup + first RFC template (2026-04-26)

**Lead:** solo bootstrap (Claude Opus 4.7 1M-context + Dipl.-Ing.(FH) Meysam Shiehzadeh)
**BUILD-PLAN row:** session 6 — "RFC repo setup + first RFC template"

### Decisions resolved

1. **Adopt the Rust language team's RFC template + RFCs README
   verbatim-with-minimal-adaptation.** Same posture as sessions 1–5
   ("adopt canonical, customise minimally, preserve attribution").
   The Rust language team's `rust-lang/rfcs` repository is the
   de-facto canonical OSS RFC pattern in 2026 — adopted by
   hundreds of derivative projects across the ecosystem — and the
   template is licensed `Apache-2.0 OR MIT`, matching this
   project's licence posture exactly. Heavier alternatives
   (full `rfcbot`-driven FCP automation; Linux-kernel-style
   patch-based RFC threads on a mailing list) are appropriate for
   larger projects with multiple sub-teams; the Rust template +
   process README is the right scaffold for a single-maintainer
   pre-alpha with a clear path to multi-reviewer and TSC governance.
   Nothing reinvented; project-specific tailoring is the
   comment-window matrix (matched to GOVERNANCE.md decision
   categories), the explicit RFC → ADR back-fill rule, the
   BUILD-PLAN/gate-19 framing, and the Superglue-Pro-specific
   wording in the template body.
2. **RFC repo location: `supergluepro/rfcs`, public from day one.**
   BUILD-PLAN row 6 mandates it. Created via `gh repo create
   supergluepro/rfcs --public --description "…" --source=. --remote=origin
   --push` against an already-authenticated `gh` session
   (account `veritarium`, scopes `gist read:org repo`, SSH transport).
   No `gh auth login` was invoked, honouring the auto-memory rule
   "Git transport: SSH only — never `gh auth login` or HTTPS
   remotes" for `supergluepro/*` repositories. The resulting
   remote URL is `git@github.com:supergluepro/rfcs.git`.
3. **Comment-window matrix: 10/30/30/14/5 days.** Routine
   architectural RFCs ≥10 days (per `GOVERNANCE.md § Architectural
   changes` step 2); governance-document RFCs 30 days (per
   `GOVERNANCE.md § Updating this document` step 2);
   first-additional-maintainer nomination 30 days (per
   `GOVERNANCE.md § Adding the first additional maintainer`
   step 1); subsequent maintainer nominations 14 days (per the
   role table); emergency governance change ≥5 days. Documented
   once, in the new repo's `README.md § Comment windows and
   reviewer requirements`. The meta-repo's `CONTRIBUTING.md § RFCs`
   now links to that anchor rather than duplicating the matrix.
4. **FCP duration: 7 days, manually announced.** A maintainer
   announces final-comment period in a PR comment with a stated
   disposition (merge / close / postpone). 7-day clock runs
   concurrently with no further design changes. Substantial new
   arguments cancel FCP and the RFC returns to development.
   Manual announcement is appropriate at single-maintainer
   pre-alpha; if the project later wants `rfcbot`-style automation,
   that lands via an RFC against this very repo (not in this
   session). Total minimum time from PR-open to merge for a
   routine architectural RFC: 10 days (comment) + 7 days (FCP) =
   17 days, assuming no extensions.
5. **Cross-specialty reviewer requirement (gate 2):
   substantively binding once Reviewers join the role ladder;
   symbolic during single-maintainer bootstrap.** Documented
   explicitly in the new repo's README (matrix footnote +
   role-ladder pointer back to `GOVERNANCE.md`). Same posture as
   gate 17 (48 h public review) — a gate that exists from session
   1 but is symbolic with one contributor and substantive once a
   second maintainer is added.
6. **DCO workflow mirrored verbatim, not abstracted.** The
   `.github/workflows/dco.yml` in the new repo is a byte-for-byte
   copy of the meta-repo's workflow (same `actions/checkout@v4`
   reference, same TODO-session-32 SHA-pinning marker). Sign-off
   enforcement is parity-equivalent across `supergluepro/*`. A
   future cross-repo abstraction (reusable workflow under
   `supergluepro/.github`?) is a candidate for session 32 alongside
   the SHA-pinning Renovate work, but not in this session — copying
   is the right move at the current scale.
7. **Per-repo doc layout: no separate `MAINTAINERS`,
   `CODE_OF_CONDUCT.md`, `SECURITY.md`, or `GOVERNANCE.md` in
   `supergluepro/rfcs`.** Org-wide governance / CoC / disclosure /
   maintainer roster live in the meta-repo and are linked from
   the new repo's README and CONTRIBUTING.md. When the
   production-code repo flips public at end of Phase A, it
   carries its own CONTRIBUTING.md (Rust-toolchain-specific) but
   inherits the meta-repo's governance / CoC / SECURITY documents
   the same way.
8. **`GOVERNANCE.md`: no edits this session.** Session 5's
   deferred follow-up explicitly said "No file edit needed in
   `GOVERNANCE.md` itself; the path works whether or not the repo
   is created yet (broken link until then, by design of the
   BUILD-PLAN sequencing)." That guidance is followed here. The
   parenthetical "(once that repo is created in BUILD-PLAN
   session 6)" hedges in `§ Adding the first additional maintainer`
   step 1 and `§ Architectural changes` step 1 remain in place
   as time-stamped historical context — they become charming
   anchors rather than actively misleading wording once session 6
   ships. Cleaning them up would be a wording-only edit to
   `GOVERNANCE.md`, which strictly requires an RFC + 30-day window
   per the higher-bar self-update process; deferred until a
   substantive GOVERNANCE.md RFC opens for unrelated reasons.
9. **Branch protection on the new repo: deferred to BUILD-PLAN
   session 18.** Session 18 lands branch protection + signed
   commits (`gitsign`, keyless OIDC) across all `supergluepro/*`
   repos at once. Single-maintainer bootstrap can self-discipline
   until then; the DCO workflow already gates PR commits.
10. **No ADR planned by default for session 6.** Same posture as
    sessions 3, 4, 5: the choice of the Rust template + Rust RFCs
    README adapted with a Superglue-Pro-specific window matrix is
    well-grounded in canonical OSS practice and BUILD-PLAN row 6,
    not a controversial architectural decision. The maintainer
    may choose to record `ADR-008 — RFC repo + process: Rust-
    language-team-style adaptation with Superglue-Pro-specific
    comment-window matrix` in session 7 alongside the planned
    ADR-001..006 back-fills (and optional ADR-007 governance) if
    the format invites a "why this not that" entry. Otherwise the
    rationale stays in this log + the new repo's README.

### Files

| Action | Path | Note |
|---|---|---|
| add (new repo) | `supergluepro/rfcs README.md` | RFC process documentation. Adapts the Rust RFCs README with Superglue-Pro-specific deltas (window matrix, RFC → ADR back-fill rule, BUILD-PLAN/gate-19 framing, no `rfcbot` automation). |
| add (new repo) | `supergluepro/rfcs text/0000-template.md` | RFC template adapted from `rust-lang/rfcs/0000-template.md`. Header refs flipped to `supergluepro/rfcs` and `supergluepro/superglue`; body framing localised; section structure preserved. |
| add (new repo) | `supergluepro/rfcs CONTRIBUTING.md` | Short discoverability stub pointing at README and the meta-repo's `CONTRIBUTING.md` for org-wide DCO + Conventional Commits + CoC + security norms. |
| add (new repo) | `supergluepro/rfcs LICENSE-MIT, LICENSE-APACHE, NOTICE` | Dual MIT OR Apache-2.0; copyright Dipl.-Ing.(FH) Meysam Shiehzadeh, 2026. NOTICE attributes the adapted README + RFC template to `rust-lang/rfcs`. |
| add (new repo) | `supergluepro/rfcs .github/workflows/dco.yml` | DCO sign-off enforcement, mirrored verbatim from meta-repo (including the TODO-session-32 SHA-pinning marker). |
| add (new repo) | `supergluepro/rfcs .gitignore` | Minimal — OS / editor / local env / `.claude/settings.local.json`. No Rust patterns. |
| modify | `README.md` | § What lives in this repo bullet flipped from "(populated from session 6 onward)" to live link + pointer to the new repo's process README. § Contributing parenthetical "(populated from session 6)" removed from the inline RFC reference. |
| modify | `BUILD-PLAN.md` | § What lives where table — `supergluepro/rfcs` row's Visibility flipped from "Public (created in session 6)" to "Public"; Purpose cell expanded with a pointer to the new repo's process README. No row added/removed; no scheduling changes. |
| modify | `CONTRIBUTING.md` | § Where things live table — `supergluepro/rfcs` Visibility flipped to "Public". § RFCs paragraph replaced — placeholder removed; live links to the new repo's process README, comment-window matrix anchor, and template. |
| modify | `CHANGELOG.md` | + Added: new repo `supergluepro/rfcs` entry. + Decided: 10 entries (RFC repo location, RFC template source, RFC process README source, FCP duration, DCO mirroring, license, cross-specialty reviewer posture, per-repo doc layout, ADR back-fill posture, GOVERNANCE.md non-edit). + Changed: README.md, BUILD-PLAN.md, CONTRIBUTING.md cross-reference flips. |
| modify | `SESSION-LOG.md` | This entry. |

### Per-session gate sweep

| # | Gate | Status |
|---:|---|---|
| 11 | License + supply-chain green | ✅ The only "dependency" introduced is the Rust RFCs template + README (Apache-2.0 OR MIT, identical licence posture). Compatible side-by-side with `MIT OR Apache-2.0` because distinct documents under matching licences are independently distributable. Attribution preserved in `NOTICE` and `README § Attribution` of the new repo. |
| 13 | CHANGELOG.md updated | ✅ Added / Decided / Changed entries land alongside files. |
| 16 | Glue-only honored | ✅ Rust template + RFCs README adopted with minimal adaptation; nothing parallel reinvented. The DCO workflow is mirrored verbatim from the meta-repo (also previously hand-rolled per session 2's gate 16). |
| 1, 2, 3, 4, 17 | Expert / reviewers / RFC / ADR / 48 h public review | ⚠️ Symbolic during bootstrap, same posture as sessions 1–5. RFC repo now exists (this is its bootstrap session); ADR format not built yet (session 7). 48 h public review symbolic on a meta-repo with one contributor; the same self-discipline posture continues. The new repo's first PR — when an external contributor opens an RFC — is the first time gates 1, 2, 17 will run substantively against this surface. |
| 5–10, 12, 14–15, 18–19 | Test/perf/repro/SemVer/security/SBOM/random-repo | N/A — no Rust code, no CI matrix, no release, no fixture corpus at this stage. The DCO workflow on the new repo runs live on its first PR. |

### Deferred follow-ups

- **Session 7 (next) — ADR format + first 6 ADRs documenting current
  architecture.** Michael Nygard format. ADR-001 license,
  ADR-002 brand, ADR-003 DCO over CLA, ADR-004 in-repo Action
  over Probot, ADR-005 MCP as v1.0 architecture, ADR-006 Claude
  Code as primary consumer. Maintainer's discretion on
  `ADR-007 — Governance shape: single-maintainer with CNCF MVG-style
  TSC path` (carried over from session 5) and `ADR-008 — RFC repo +
  process: Rust-language-team-style adaptation with Superglue-Pro-
  specific comment-window matrix` alongside. Once `/adr/` exists,
  every "RFC → ADR back-fill" pointer in the new repo's README
  becomes a live, testable cross-reference. The new repo's README
  references `/adr/` as "(created in BUILD-PLAN session 7)" — that
  hedge becomes stale next session and should be cleaned up in the
  README at that time (the new repo's README is not under the
  governance higher-bar process; routine PR + 48 h public review
  is sufficient).
- **Session 8 — PR + code-review templates.** Should reference
  the new RFC repo's process README (and especially the
  comment-window matrix anchor) in addition to the BUILD-PLAN
  19-gate list. The PR-template flowchart for "do I need an RFC?"
  becomes a live link to the matrix.
- **Session 13 / first architectural RFC.** Session 13 freezes
  the Claude-Code-first CLI surface as a v0 contract via an RFC
  + ADR-006 expansion. That session is the first time the
  10-day comment window + 7-day FCP run live in
  `supergluepro/rfcs`. Until then, the repo holds only the
  template + process docs. Session 13 will likely surface UX
  refinements to the new repo's process README — capture them
  in a session-13 follow-up edit.
- **Session 18 — branch protection + signed commits.** Adds
  `gitsign` + `main`-required-status-checks across every
  `supergluepro/*` repo, including `supergluepro/rfcs`. Until
  then, single-maintainer self-discipline is the floor.
- **Session 20 — Renovate.** Once enabled, the
  `actions/checkout@v4` reference in the new repo's
  `dco.yml` (and the meta-repo's identical workflow) gets
  digest-pinned auto-PRs.
- **Session 32 — OpenSSF Scorecard hardening.** SHA-pin
  third-party Actions in both `supergluepro/supergluepro` and
  `supergluepro/rfcs` (and any other `supergluepro/*` repos by
  then). Replace `actions/checkout@v4` with
  `actions/checkout@<sha> # v4.x.x` in both
  `dco.yml` files.
- **Cross-repo workflow abstraction (post-Phase B).** When more
  than two `supergluepro/*` repos run identical workflows
  (DCO check, SBOM upload, etc.), consider a reusable workflow
  under `supergluepro/.github`. Not pressing at session 6's
  scale; revisit when ≥4 repos exist.
- **End of Phase A (when `supergluepro/superglue` flips public):**
  the production-code repo carries its own `CONTRIBUTING.md`
  (Rust-toolchain-specific) and inherits the meta-repo's
  governance / CoC / SECURITY documents the same way the new
  RFCs repo does. The new RFCs repo continues to host all
  architectural-RFC discussions for the production-code repo —
  i.e. proposing a new signal extractor or output-schema
  extension lives in `supergluepro/rfcs`, not in
  `supergluepro/superglue`.
- **`MAINTAINERS` file (when first additional maintainer
  onboards):** lands in the meta-repo at that point;
  `supergluepro/rfcs` and other org repos point at it via a
  one-line README pointer. No need to duplicate per repo.

### State for the next session

Tracked tree at the end of session 6 — **two repos now**:

```
# supergluepro/supergluepro (this meta-repo)
.github/
  workflows/
    dco.yml          — DCO Signed-off-by enforcement on every PR
.gitignore           — standard + .claude/settings.local.json + user-text.txt excluded
BUILD-PLAN.md        — v10, "Resolved decisions" section, supergluepro/rfcs row updated to live
CHANGELOG.md         — Keep-a-Changelog 1.1.0; sessions 1+2+3+4+5+6 entries under [Unreleased]
CODE_OF_CONDUCT.md   — Contributor Covenant 3.0 verbatim; § Reporting links to SECURITY.md
CONTRIBUTING.md      — DCO + Conventional Commits + Governance pointer; § RFCs links to supergluepro/rfcs process README
GOVERNANCE.md        — role ladder, decision making, TSC formation path, succession & continuity, CoI, higher-bar self-update; CNCF MVG + Project Template scaffold (UNCHANGED in session 6)
LICENSE-APACHE       — Apache 2.0 full text
LICENSE-MIT          — MIT
NOTICE               — minimal Apache-style notice
README.md            — License + Contributing pointer; supergluepro/rfcs reference now live
SECURITY.md          — GPVR primary, email backup, PGP placeholder; 90-day window; CVSS v4.0; CVE via GitHub CNA
SESSION-LOG.md       — sessions 1+2+3+4+5+6
THREAT-MODEL.md      — v0.1 STRIDE-lite per surface

# supergluepro/rfcs (NEW)
.github/
  workflows/
    dco.yml          — DCO Signed-off-by enforcement on every PR (mirrored from meta-repo)
.gitignore           — minimal (markdown-only repo)
CONTRIBUTING.md      — short discoverability stub
LICENSE-APACHE       — Apache 2.0 full text
LICENSE-MIT          — MIT
NOTICE               — dual-licence + attribution to rust-lang/rfcs
README.md            — RFC process documentation (window matrix, FCP, RFC → ADR back-fill)
text/
  0000-template.md   — RFC template (adapted from rust-lang/rfcs)
```

Untracked / gitignored in the meta-repo: `.claude/settings.local.json`
and `user-text.txt`. The `supergluepro/rfcs` working tree is at
`C:/Users/meysr/Projects/supergluepro-rfcs/` locally (root-commit
`4c73e91` direct-to-`main`, signed off, pushed via SSH).

Session 7 (ADR format + first 6 ADRs documenting current architecture)
is the next BUILD-PLAN row. Michael Nygard format. Deliverables to
scope at the top of session 7:

- Create `/adr/` directory in this meta-repo with `adr-template.md`
  (Nygard format: status, context, decision, consequences).
- Write **ADR-001** (license = `MIT OR Apache-2.0` dual; from
  session 1).
- Write **ADR-002** (brand = unregistered, anchored by domain +
  GitHub org; from session 1).
- Write **ADR-003** (DCO over CLA; from session 2).
- Write **ADR-004** (in-repo workflow over Probot DCO app; from
  session 2).
- Write **ADR-005** (MCP as v1.0 architecture; existing project-wide
  invariant from BUILD-PLAN).
- Write **ADR-006** (Claude Code as primary consumer; existing
  project-wide invariant from BUILD-PLAN).
- Maintainer's discretion: **ADR-007** (Governance shape:
  single-maintainer with CNCF MVG-style TSC path; from session 5)
  and **ADR-008** (RFC repo + process: Rust-language-team-style
  adaptation with Superglue-Pro-specific comment-window matrix;
  from session 6) alongside.
- Cross-link from `BUILD-PLAN.md`, `CONTRIBUTING.md`,
  `GOVERNANCE.md`, and the new RFCs repo's `README.md` — every
  "RFC → ADR back-fill" pointer becomes live; the "(created in
  BUILD-PLAN session 7)" hedges in `CONTRIBUTING.md § ADRs` and
  the new RFCs repo's README are flipped from forward-pointer to
  live link. The RFCs repo's README is not under the governance
  higher-bar process, so its hedges can be cleaned up via a
  routine PR + 48 h public review at that time.

Once session 7 ships, every cross-document ADR pointer in this repo
flips from "(once /adr/ is created in session 7)" to a live link.

---

## Session 5 — GOVERNANCE.md + steering structure (2026-04-26)

**Lead:** solo bootstrap (Claude Opus 4.7 1M-context + Dipl.-Ing.(FH) Meysam Shiehzadeh)
**BUILD-PLAN row:** session 5 — "GOVERNANCE.md + steering structure"

### Decisions resolved

1. **Structural skeleton: CNCF Project Template + Minimum Viable
   Governance v1 (MVG).** Same posture as sessions 1–4 ("adopt
   canonical, customise minimally, preserve attribution") — both are
   Apache-2.0 / CC-BY-4.0, used by Kubernetes / Envoy / Prometheus /
   OpenTelemetry / Sigstore, and explicitly intended for downstream
   adoption with project-specific tailoring. Heavier alternatives
   (Apache Software Foundation PMC/IPMC, Rust Project Leadership
   Council + Teams) are appropriate for graduated, multi-org-funded
   projects; CNCF MVG is the right scaffold for a single-maintainer
   pre-alpha with a clear path to formal governance. No GOVERNANCE.md
   was reinvented from scratch; the project-specific tailoring is the
   TSC trigger threshold (BUILD-PLAN-mandated), the role-ladder
   description, and the succession plan.
2. **TSC trigger threshold: ≥3 active maintainers in the prior 6
   months.** The threshold itself is mandated by BUILD-PLAN row 5
   ("≥3 maintainers triggers TSC formation"). The active-window
   qualification is the CNCF norm and makes the trigger objectively
   measurable rather than a head-count of past appointees who may have
   gone inactive. Active = at least one substantive contribution,
   review, or RFC engagement in the prior 6 months.
3. **Role ladder: User → Contributor → Reviewer → Committer →
   Maintainer → TSC (latent).** The CNCF five-plus-one ladder.
   Documented up front so future contributors can step into them
   naturally; at single-maintainer pre-alpha, the four upper rungs are
   filled by the same person. Each rung has explicit promotion
   criteria (sustained activity time + nomination + approval window +
   approval threshold).
4. **First-additional-maintainer transition gets extra ceremony.**
   Subsequent maintainer additions follow the standard 14-day RFC
   window; the *first* additional maintainer gets a 30-day window plus
   a published transition plan (which repos receive merge rights
   immediately, which require a probation period). Reasoning: the
   transition from one maintainer to two is the most consequential in
   the project's life — it is the point at which gate 17 (48 h public
   review window) becomes substantively binding rather than symbolic
   self-discipline, and the point at which decision-making moves from
   "the founding maintainer chooses" to "two people must agree."
5. **TSC default composition: 5 seats, 1-year staggered terms,
   ⅔-supermajority for governance / security / license / brand / CNCF
   decisions, simple majority for routine motions, ≤⅔ from a single
   employer.** Standard CNCF defaults; revisitable by RFC once the
   trigger fires and real composition is being decided. The single
   most important constraint is the single-employer cap to keep
   decision-making independent of any single sponsor.
6. **Succession & continuity posture: pragmatic, document the
   practical chain.** Domain transfer via united-domains GmbH (DE)
   standard procedures (auth code retrieval, transfer to a successor's
   chosen registrar; next-of-kin or estate executor contacts the
   registrar with proof of authority). GitHub organisation transfer
   per [GitHub's deceased-user policy](https://docs.github.com/en/site-policy/other-site-policies/github-deceased-user-policy)
   rather than credential recovery. **Dual `MIT OR Apache-2.0` license
   means any contributor may fork without permission** — this is the
   ultimate safety net. Pre-`v0.1.0` no signing keys exist; once
   BUILD-PLAN session 25 ships Sigstore-keyless OIDC, there is no
   long-lived secret to transfer (by design of the keyless flow).
7. **Conflict-of-interest disclosure scope: three categories.**
   Employer / primary contracting relationship; substantial financial
   relationship with a downstream consumer (>USD 1,000/month or >0.1%
   equity as a default soft threshold); authorship or maintenance of a
   directly-competing tool (Repomix, gitingest, Aider repo-map,
   similar code-context extractors). Disclosures live publicly in
   GOVERNANCE.md § Current roster (or in `MAINTAINERS` once that file
   exists). Recusal expected for votes where the conflict applies; a
   recused vote does not count toward quorum or supermajority
   denominators.
8. **GOVERNANCE.md self-update process: higher bar than other
   public-surface changes.** RFC + **30-day window** (3× the routine
   10-day RFC window — governance changes deserve more time to surface
   concerns) + (until TSC seated) all maintainers approve, (once TSC
   seated) TSC supermajority with recused votes excluded from the
   denominator. Emergency exceptions (security / legal) may use a
   shortened ≥5-day window with public reasoning attached to the RFC,
   ratified or reverted at the next governance review.
9. **`MAINTAINERS` file deferred until the first additional
   maintainer.** BUILD-PLAN row 5 doesn't mandate a separate
   `MAINTAINERS` file; until there is a second name to roster,
   splitting it out from GOVERNANCE.md § Current roster would be
   over-architecture. When the first additional maintainer is
   onboarded, the roster moves into a dedicated `MAINTAINERS` file
   and GOVERNANCE.md § Current roster becomes a one-line pointer.
10. **Trademark / brand posture inherits BUILD-PLAN row 1.** No
    registered trademark; brand anchored by domain + GitHub org +
    consistent display-name use. New addition: a good-faith
    no-confusing-fork-naming request (forks are permitted under the
    dual MIT/Apache license but are asked not to use "Superglue Pro"
    as their primary brand name to avoid downstream confusion). This
    is a request rooted in good-faith branding norms, not a legal
    restriction; the dual license itself imposes no naming constraint.
11. **No `CODE_OF_CONDUCT.md` or `SECURITY.md` edits this session.**
    The session-3 follow-up on `CODE_OF_CONDUCT.md § Reporting an
    Issue` and the session-4 follow-up on `SECURITY.md` triage-group
    references both said *"becomes actionable in session 5 only if a
    TSC is seated then; otherwise the bootstrap-maintainer language
    continues to be correct."* No TSC is seated this session
    (single-maintainer pre-alpha), so the bootstrap language remains
    correct in both files. They do not require an edit; their
    existing forward-pointers continue to work.
12. **No ADR planned by default for session 5.** Same posture as
    sessions 3 and 4: governance shape is well-grounded in the CNCF
    MVG canonical pattern + BUILD-PLAN row 5's threshold, not a
    controversial architectural decision. The maintainer may choose to
    record `ADR-007 — Governance shape: single-maintainer with CNCF
    MVG-style TSC path` in session 7 alongside the planned ADR-001..006
    back-fills if the format invites it. Otherwise the rationale stays
    in this log + GOVERNANCE.md.

### Files

| Action | Path | Note |
|---|---|---|
| add | `GOVERNANCE.md` | Role ladder (User → Contributor → Reviewer → Committer → Maintainer → TSC, the latter latent); decision making (routine PR / architectural-RFC / public-surface / governance / CoC enforcement / security disclosure); TSC formation path (≥3 active maintainers in prior 6 months trigger; 5 seats odd-sized; 1-year staggered terms; ≤⅔ from a single employer; supermajority thresholds); succession & continuity (domain transfer via united-domains; GitHub org-transfer per GitHub deceased-user policy; dual-license fork-friendliness as the ultimate safety net); conflict-of-interest disclosure (3 categories: employer, downstream-consumer financial relationship >USD 1,000/mo or >0.1% equity, competing-tool authorship); CoC pointer (CV 3.0 + Community Moderators role inheritance); trademark posture (BUILD-PLAN inheritance + good-faith no-confusing-fork-naming request); higher-bar self-update process (RFC + 30-day window + supermajority-once-seated). Adapts CNCF Project Template + MVG v1; attribution preserved in the document footer. |
| modify | `CONTRIBUTING.md` | + new `## Governance` section between `## ADRs` and `## Code of Conduct` pointing at `GOVERNANCE.md` and noting the TSC trigger + the higher-bar self-update process. § Submitting changes step 7 ("becomes binding once a second maintainer is added") now links to `GOVERNANCE.md § Adding the first additional maintainer`. |
| modify | `CHANGELOG.md` | + Added: `GOVERNANCE.md`. + Decided: TSC trigger threshold (≥3 active maintainers in prior 6 months); role ladder (CNCF five-plus-one); succession & continuity posture; conflict-of-interest disclosure scope; GOVERNANCE.md self-update process; ADR back-fill posture (defer to session 7); `MAINTAINERS` file deferred. + Changed: `CONTRIBUTING.md` (governance section + step 7 link). |
| modify | `SESSION-LOG.md` | This entry. |

### Per-session gate sweep

| # | Gate | Status |
|---:|---|---|
| 11 | License + supply-chain green | ✅ No new dependencies introduced. The single new doc dependency is the CNCF Project Template + MVG v1 (Apache-2.0 / CC-BY-4.0); compatible side-by-side with `MIT OR Apache-2.0` because distinct documents under distinct licences are independently distributable. Attribution preserved in the GOVERNANCE.md footer. |
| 13 | CHANGELOG.md updated | ✅ Added / Decided / Changed entries land alongside files. |
| 16 | Glue-only honored | ✅ CNCF MVG + Project Template scaffold adopted as the structural skeleton; nothing reinvented. |
| 1, 2, 3, 4, 17 | Expert / reviewers / RFC / ADR / 48 h public review | ⚠️ Symbolic during bootstrap, same posture as sessions 1–4. RFC repo not built yet (BUILD-PLAN session 6); ADR format not built yet (session 7); a session-5 ADR-007 ("Governance shape: single-maintainer with CNCF MVG-style TSC path") is at the maintainer's discretion in session 7. 48 h public-review symbolic on a meta-repo with one contributor; the same self-discipline posture applied in sessions 1–4 continues here. The 30-day governance-change window documented in GOVERNANCE.md § Updating this document begins to apply to *future* GOVERNANCE.md changes; this session establishes the document and is itself a bootstrap commit, same as the other governance docs in sessions 1–4. |
| 5–10, 12, 14–15, 18–19 | Test/perf/repro/SemVer/security/SBOM/random-repo | N/A — no Rust code, no CI matrix, no release, no fixture corpus at this stage. |

### Deferred follow-ups

- **Session 6 (next) — RFC repo + first RFC template.** Once
  `supergluepro/rfcs` exists, every "open an RFC against
  `supergluepro/rfcs`" pointer in `GOVERNANCE.md` (lines covering
  architectural changes, maintainer nominations, TSC formation, and
  GOVERNANCE.md self-updates) becomes a live link to a working
  template. No file edit needed in `GOVERNANCE.md` itself; the path
  works whether or not the repo is created yet (broken link until
  then, by design of the BUILD-PLAN sequencing).
- **Session 7 — ADR back-fills.** Maintainer's choice on whether to
  record `ADR-007 — Governance shape: single-maintainer with CNCF
  MVG-style TSC path` alongside the planned ADR-001..006 back-fills.
  Rationale already lives in this log + GOVERNANCE.md whether or not
  an ADR is created.
- **Session 8 — PR + code-review templates.** Mechanise gates 1–19
  as PR-template checklists. The PR template should reference
  `GOVERNANCE.md § Decision making` for the routine-PR / architectural
  / public-surface / governance change-type matrix in addition to the
  BUILD-PLAN gate list.
- **First additional maintainer onboarding (post-`v0.1.0` recruitment
  per BUILD-PLAN Phase B end).** When this happens, three things
  follow:
  1. RFC against `supergluepro/rfcs` with a 30-day window + transition
     plan, per GOVERNANCE.md § Adding the first additional maintainer.
  2. Roster moves out of GOVERNANCE.md § Current roster into a
     dedicated `MAINTAINERS` file; § Current roster becomes a
     one-line pointer.
  3. CONTRIBUTING.md § Submitting changes step 7 ("becomes
     substantively binding once a second maintainer is added") flips
     from forward-looking to live; gate 17 (48 h public review)
     becomes binding rather than symbolic.
- **TSC seating (≥3 active maintainers reached).** When triggered:
  1. Any existing maintainer opens an RFC against `supergluepro/rfcs`
     formally seating the TSC per GOVERNANCE.md § Trigger and formation.
  2. `CODE_OF_CONDUCT.md § Reporting an Issue` updates from
     "Community Moderators role is filled by the project maintainer"
     to "Community Moderators role is filled by the seated TSC
     (or its delegated Conduct Committee, if so resolved)."
  3. `SECURITY.md` updates from "the single maintainer triages" to
     "the seated Security Response Group triages."
  4. `MAINTAINERS` file (if not already created) lands here at the
     latest; TSC seat assignments documented.
  5. GOVERNANCE.md § Current roster updates to reflect the new
     governance shape; the "(latent)" markers on the TSC sections
     are removed.
- **End of Phase A (when `supergluepro/superglue` flips public):**
  Replicate `GOVERNANCE.md` in that repo with code-side governance
  concretised (e.g. who has merge rights on `supergluepro/superglue`
  vs `supergluepro/patterns` vs `supergluepro/fixture-corpus` once
  Reviewers / Committers exist in those repos). The meta-repo's
  document remains authoritative for project-wide governance; the
  production repo's document extends with code-specific role
  privileges.
- **Conflict-of-interest disclosure for the founding maintainer.**
  GOVERNANCE.md § Current roster currently lists only the
  maintainer's name + email. Once `MAINTAINERS` is split out, the
  roster entry should include the maintainer's primary employer or
  contracting relationship (per the COI scope decision) at the time
  of the snapshot. The maintainer self-files the disclosure when the
  `MAINTAINERS` file is created.

### State for the next session

Tracked tree at the end of session 5:

```
.github/
  workflows/
    dco.yml          — DCO Signed-off-by enforcement on every PR
.gitignore           — standard + .claude/settings.local.json + user-text.txt excluded
BUILD-PLAN.md        — v10, "Resolved decisions" section
CHANGELOG.md         — Keep-a-Changelog 1.1.0; sessions 1+2+3+4+5 entries under [Unreleased]
CODE_OF_CONDUCT.md   — Contributor Covenant 3.0 verbatim; § Reporting now links to SECURITY.md
CONTRIBUTING.md      — DCO + Conventional Commits; § Code of Conduct + § Communication link to SECURITY.md; § Governance pointing at GOVERNANCE.md
GOVERNANCE.md        — role ladder, decision making, TSC formation path, succession & continuity, conflict-of-interest, higher-bar self-update process; CNCF MVG + Project Template scaffold with attribution preserved
LICENSE-APACHE       — Apache 2.0 full text
LICENSE-MIT          — MIT
NOTICE               — minimal Apache-style notice
README.md            — License + Contributing pointer
SECURITY.md          — GPVR primary, email backup, PGP placeholder; 90-day window; CVSS v4.0; CVE via GitHub CNA; supported-versions; scope; safe harbor; supply-chain alignment + forward-pointer table
SESSION-LOG.md       — sessions 1+2+3+4+5
THREAT-MODEL.md      — v0.1 STRIDE-lite per surface (CLI, output-schema-as-LLM-input, MCP server, pattern library, fixture corpus, build/release, distribution); adversary model; asset inventory; update cadence
```

Untracked / gitignored: `.claude/settings.local.json` and `user-text.txt`.

Session 6 (RFC repo setup + first RFC template) is the next BUILD-PLAN
row. Rust-language-team alum lead. Repo: `supergluepro/rfcs`.
Deliverables to scope at the top of session 6:

- Create the `supergluepro/rfcs` GitHub repository (public from day
  one).
- Adopt the [Rust language team RFC template](https://github.com/rust-lang/rfcs/blob/master/0000-template.md)
  as the project's RFC template (CC0 / Apache-2.0 + MIT — same
  posture-fit as CNCF MVG and CV 3.0; canonical, not reinvented).
- Document the RFC process in the new repo's README: ≥10-day public
  comment window for routine architectural RFCs, 30-day window for
  governance-document RFCs (per GOVERNANCE.md § Updating this
  document), 30-day window for first-additional-maintainer RFCs, two
  reviewer approvals, the path from RFC merge to ADR back-fill.
- Cross-link from `BUILD-PLAN.md`, `README.md`, `CONTRIBUTING.md`,
  `GOVERNANCE.md` — every "open an RFC against `supergluepro/rfcs`"
  pointer becomes a live link.

Once session 6 ships, every cross-document RFC pointer in this repo
flips from "(once that repo is created in session 6)" to a live link.

---

## Session 4 — SECURITY.md + threat-model v0.1 (2026-04-26)

**Lead:** solo bootstrap (Claude Opus 4.7 1M-context + Dipl.-Ing.(FH) Meysam Shiehzadeh)
**BUILD-PLAN row:** session 4 — "SECURITY.md + threat-model v0.1"

### Decisions resolved

1. **Disclosure channel: GitHub Private Vulnerability Reporting (GPVR)
   primary, email backup, PGP key planned.** Modern 2026 OSS practice
   (Sigstore, Vue, React, Kubernetes-via-private-list, etc.) leads with
   GPVR — same surface as the code, automatic maintainer notification, no
   third-party app dependency. Email backup: `meysam@shiehzadeh.de` until
   the `security@supergluepro.com` forwarder is provisioned (depends on
   the BUILD-PLAN session 17 DNS swap off the united-domains parking
   nameservers). PGP key on `meysam@shiehzadeh.de` is **planned but not
   gating this session**: cryptographic key material must be generated on
   the maintainer's hardware so the private half never leaves it; the AI
   agent in this session cannot perform that generation. SECURITY.md
   ships with a PGP placeholder block; the published key is a deferred
   follow-up tracked below. GPVR is itself encrypted in transit and at
   rest by GitHub, so the placeholder does not leave reporters without an
   encrypted channel.
2. **Disclosure window: 90 days.** Project Zero norm + CISA Coordinated
   Vulnerability Disclosure guidance. We publish on day 90 even if a fix
   is incomplete when continued silence creates more risk than disclosure.
3. **Severity scoring: CVSS v4.0** (FIRST, November 2023; the default
   industry standard in 2026, supersedes v3.1). v3.1 vector additionally
   provided where a consuming ecosystem requires it.
4. **CVE coordination via GitHub as a CNA.** GitHub has been a CVE
   Numbering Authority since 2018; CVE IDs are requested through the
   GitHub Security Advisory workflow during fix development. No separate
   CNA registration needed.
5. **Threat-model file location: `THREAT-MODEL.md` at repo root.**
   Parallels `SECURITY.md`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`,
   `LICENSE-*`. BUILD-PLAN doesn't dictate; the choice maximises
   discoverability at this stage. May move under `/docs/` when `/docs/`
   is built out alongside the style guide + PR templates in BUILD-PLAN
   session 8; if so, `SECURITY.md`'s relative link is updated.
6. **Threat-model methodology: STRIDE-lite per surface, pragmatic.**
   Most surfaces are not built yet (`v0.0.1-alpha` is BUILD-PLAN session
   14); each surface is marked planned-for-session-N. The novel surface
   is **"output schema as LLM input"** — Surface 2 in the document — which
   is not a traditional file-format threat surface and warrants explicit
   prompt-injection discussion. Adversary model includes opportunistic
   mass attacker, targeted attacker against a downstream consumer (the
   amplification path through Claude Code's context window),
   malicious contributor, and compromised maintainer credential.
7. **Supply-chain alignment with Rust Foundation Security Initiative
   (RFSI)** as ambient discipline (BUILD-PLAN Phase I). Typomania
   (typosquatting), crate-quarantine RFC, provenance tracking — all
   named in BUILD-PLAN row 4 — are tracked and adopted as the upstream
   outputs ship. SECURITY.md § Supply-chain alignment lists them
   explicitly alongside the BUILD-PLAN-scheduled controls
   (sessions 18, 20, 21, 23, 24, 25, 26, 32, 36, 139).
8. **No `SECURITY-INSIGHTS.yml` in this session.** OpenSSF Security
   Insights v2.x (machine-readable security metadata) is the right
   vehicle for OpenSSF Scorecard scoring (gate 32 → BUILD-PLAN session
   32). Adding it now without the rest of the Scorecard hardening
   (signed commits, SHA-pinned actions, dependency-update bot, CodeQL)
   would be premature; tracked as a deferred follow-up.

### Files

| Action | Path | Note |
|---|---|---|
| add | `SECURITY.md` | Vulnerability disclosure policy. GPVR primary, email backup, PGP placeholder; what to include; 3 business-day acknowledgement / 10 business-day triage; 90-day disclosure timeline; CVSS v4.0; CVE via GitHub CNA; supported-versions matrix; scope; safe harbor; supply-chain alignment with RFSI + forward-pointer table to BUILD-PLAN sessions 18 / 20 / 21 / 23 / 24 / 25 / 26 / 32 / 36 / 139; pointer to `THREAT-MODEL.md`; hall of fame stub. |
| add | `THREAT-MODEL.md` | v0.1 STRIDE-lite per surface (CLI consumer, output-schema-as-agent-input, MCP server, pattern library, fixture corpus, build/release pipeline, distribution paths). Adversary model + asset inventory + cross-cutting controls + out-of-scope-for-v0.1 list + update-cadence table at the end. |
| modify | `CODE_OF_CONDUCT.md` | § Reporting an Issue: forward-pointer ("PGP-encrypted disclosure channel … will be added in upcoming governance sessions") replaced with a live link to `SECURITY.md` for confidential reporting; the Community-Moderators-group pointer (BUILD-PLAN session 5, `GOVERNANCE.md`) preserved. |
| modify | `CONTRIBUTING.md` | § Code of Conduct: forward-pointer ("PGP-encrypted disclosure channel will follow in `SECURITY.md` (BUILD-PLAN session 4)") replaced with a live link. § Communication: split the email-as-security-channel stand-in into a dedicated "Security disclosures → `SECURITY.md`" bullet plus a `meysam@shiehzadeh.de`-as-CoC-contact bullet. |
| modify | `CHANGELOG.md` | + Added: `SECURITY.md`, `THREAT-MODEL.md`. + Decided: GPVR-primary disclosure, 90-day window, CVSS v4.0, GitHub-CNA, root `THREAT-MODEL.md`, STRIDE-lite, defer `SECURITY-INSIGHTS.yml` to session 32, RFSI alignment as ambient discipline. + Changed: `CODE_OF_CONDUCT.md` and `CONTRIBUTING.md` pointer updates. |
| modify | `SESSION-LOG.md` | This entry. |

### Per-session gate sweep

| # | Gate | Status |
|---:|---|---|
| 11 | License + supply-chain green | ✅ No new dependencies introduced. |
| 12 | Security checklist / SECURITY.md updated | ✅ This **is** the SECURITY.md session. Disclosure surface defined for the first time; subsequent sessions inherit gate 12 as "if your change shifts a surface tracked in `THREAT-MODEL.md`, bump that document's version and re-summarise." |
| 13 | CHANGELOG.md updated | ✅ Added / Decided / Changed entries land alongside files. |
| 16 | Glue-only honored | ✅ Adopting GPVR / CVSS v4.0 / CVE-via-GitHub / RFSI as canonical pieces; STRIDE for the threat-model methodology; nothing reinvented. |
| 1, 2, 3, 4, 17 | Expert / reviewers / RFC / ADR / 48 h public review | ⚠️ Symbolic during bootstrap, same posture as sessions 1+2+3. RFC repo not built yet (BUILD-PLAN session 6); ADR format not built yet (session 7); a session-4 ADR ("Disclosure posture: GPVR primary + 90-day window" or "Threat-model methodology: STRIDE-lite") is at the maintainer's discretion in session 7 — no separate decision rises to ADR-tier on its own, but a forward-looking entry alongside ADR-001..006 would be reasonable. 48 h public-review symbolic on a meta-repo with one contributor. |
| 5–10, 14–15, 18–19 | Test/perf/repro/SemVer/SBOM/random-repo | N/A — no Rust code, no CI matrix, no release, no fixture corpus at this stage. |

### Deferred follow-ups

- **PGP key generation (carry-over from BUILD-PLAN row 4).** The
  maintainer must generate a PGP key on `meysam@shiehzadeh.de` on their
  own hardware (passphrase choice, entropy sources, secure storage of
  the private key — operations the AI agent cannot perform on the
  maintainer's behalf). Once generated:
  - Publish the public key to `keys.openpgp.org`.
  - Add the public key to the maintainer's GitHub GPG settings (so
    commits can be GPG-signed independently of the `gitsign` work in
    BUILD-PLAN session 18).
  - Update `SECURITY.md` § PGP with the fingerprint, the keyserver URL,
    the `github.com/<handle>.gpg` URL, and the inline ASCII-armored
    public key block.
  - Update `CHANGELOG.md` Unreleased § Added with the publication entry.
  Candidate for `/schedule` background-agent reminder if the maintainer
  wants a one-time nudge in 1–2 weeks.
- **`security@supergluepro.com` forwarder provisioning.** united-domains
  panel supports email forwarding on `supergluepro.com`. Configure
  `security@` → `meysam@shiehzadeh.de` once the BUILD-PLAN session 17
  DNS swap is performed (or earlier, if forwarding is configurable
  while DNS is still on the parking nameservers). Until then,
  SECURITY.md lists `meysam@shiehzadeh.de` directly as the email backup.
  Operational, not session-scoped.
- **Session 5 (next) — `GOVERNANCE.md`.** Documents the TSC promotion
  path (≥3 maintainers triggers TSC formation per BUILD-PLAN row 5).
  When a TSC is later seated, update **CODE_OF_CONDUCT.md § Reporting an
  Issue** to refer to the seated Community Moderators group rather than
  the single maintainer. No file edit needed in session 5 itself unless
  a TSC is seated then; the bootstrap-maintainer language is correct
  until that happens.
- **Session 7 — ADR back-fills.** Maintainer's choice on whether to
  record "Disclosure posture: GPVR primary + 90-day window + CVSS v4.0
  + GitHub-CNA" or "Threat-model methodology: STRIDE-lite per surface"
  as ADR-007 / ADR-008 alongside the planned ADR-001 (license),
  ADR-002 (brand), ADR-003 (DCO over CLA), ADR-004 (in-repo Action over
  Probot DCO), ADR-005 (MCP as v1.0 architecture), ADR-006 (Claude Code
  as primary consumer). Rationale already lives in this log +
  `SECURITY.md` + `THREAT-MODEL.md` whether or not an ADR is created.
- **Session 18 — Sigstore `gitsign` for signed commits.** Once live,
  `SECURITY.md` § Supply-chain forward-pointer table updates from
  "planned" to "live."
- **Session 20 — `cargo audit` / `cargo deny` / `cargo vet` + Renovate.**
  Same flip "planned" → "live" in `SECURITY.md` § Supply-chain table.
- **Session 21 — CodeQL.** Same.
- **Session 23 — `cargo fuzz`.** Same; also folds into `THREAT-MODEL.md`
  Surface 1 mitigations (currently noted as "planned, sessions 23, 29").
- **Session 24 — SLSA L3 + CycloneDX 1.6 SBOM.** Same.
- **Session 25 — Sigstore-signed releases + crates.io Trusted
  Publishing.** Same; also folds into `THREAT-MODEL.md` Surface 6.
- **Session 32 — OpenSSF Scorecard ≥9.5 hardening.** `SECURITY-INSIGHTS.yml`
  (machine-readable security metadata) lands here, alongside SHA-pinned
  third-party actions. `THREAT-MODEL.md` bumps to a new version
  reflecting the hardened build pipeline.
- **End of Phase A (when `supergluepro/superglue` flips public):**
  Replicate `SECURITY.md` and `THREAT-MODEL.md` in that repo with
  code-side surfaces concretised. The meta-repo's `THREAT-MODEL.md`
  references mostly planned-for-session-N surfaces; the production repo
  will have most of them built by then. The meta-repo's documents
  remain authoritative for governance / disclosure policy; the
  production repo's documents extend with code-specific threat surfaces
  (specific signal extractors, ast-grep matcher engine, walker bounds
  enforcement).
- **Phase H end (sessions 129, 130, 132, 136 onward):** Add the WASM
  playground, embeddings model, LLM-judge model, and cross-repo
  federation surfaces to `THREAT-MODEL.md` as they ship. Bump to v2.0
  alongside the `v2.0.0` release tag.

### State for the next session

Tracked tree at the end of session 4:

```
.github/
  workflows/
    dco.yml          — DCO Signed-off-by enforcement on every PR
.gitignore           — standard + .claude/settings.local.json + user-text.txt excluded
BUILD-PLAN.md        — v10, "Resolved decisions" section
CHANGELOG.md         — Keep-a-Changelog 1.1.0; sessions 1+2+3+4 entries under [Unreleased]
CODE_OF_CONDUCT.md   — Contributor Covenant 3.0 verbatim; § Reporting now links to SECURITY.md
CONTRIBUTING.md      — DCO + Conventional Commits; § Code of Conduct + § Communication now link to SECURITY.md
LICENSE-APACHE       — Apache 2.0 full text
LICENSE-MIT          — MIT
NOTICE               — minimal Apache-style notice
README.md            — License + Contributing pointer
SECURITY.md          — Vulnerability disclosure policy: GPVR primary, email backup, PGP placeholder; 90-day window; CVSS v4.0; CVE via GitHub CNA; supported-versions; scope; safe harbor; supply-chain alignment + forward-pointer table
SESSION-LOG.md       — sessions 1+2+3+4
THREAT-MODEL.md      — v0.1 STRIDE-lite per surface (CLI, output-schema-as-LLM-input, MCP server, pattern library, fixture corpus, build/release, distribution); adversary model; asset inventory; update cadence
```

Untracked / gitignored: `.claude/settings.local.json` and `user-text.txt`.

Session 5 (`GOVERNANCE.md` + steering structure) is the next BUILD-PLAN
row. Single-maintainer with documented TSC promotion path (≥3 maintainers
triggers TSC formation per BUILD-PLAN row 5). Deliverables to scope at the
top of session 5:

- `GOVERNANCE.md` documenting decision-making, role definitions
  (maintainer, committer, reviewer), the TSC promotion path, succession
  plan, and conflict-of-interest disclosure norms.
- Cross-file pointer back-links once roles are defined: CODE_OF_CONDUCT.md
  § Reporting an Issue (Community Moderators group becomes a real
  reference if a TSC is seated then), SECURITY.md (triage group reference
  if applicable), CONTRIBUTING.md § Where things live (TSC table row
  added when applicable).
- The session-3 follow-up about CODE_OF_CONDUCT.md § Reporting referring
  to the seated Community Moderators group becomes actionable in
  session 5 **only if** a TSC is seated then; otherwise the
  bootstrap-maintainer language continues to be correct.

---

## Session 3 — CODE_OF_CONDUCT.md (Contributor Covenant 3.0) (2026-04-26)

**Lead:** solo bootstrap (Claude Opus 4.7 1M-context + Dipl.-Ing.(FH) Meysam Shiehzadeh)
**BUILD-PLAN row:** session 3 — "CODE_OF_CONDUCT.md (Contributor Covenant 3.0)"

### Decisions resolved

1. **Adopt Contributor Covenant 3.0 verbatim from the canonical source**
   (`https://www.contributor-covenant.org/version/3/0/code_of_conduct/code_of_conduct.md`).
   Don't fork, rewrite, or paraphrase. CV 3.0 is the de facto OSS code of
   conduct in 2026 — adopted by the Linux Foundation, Django (2026-04-15),
   Rails, Kubernetes, etc. — and reorganises the prior v2.1 into the
   "Encouraged Behaviors" / "Restricted Behaviors" framing plus a four-rung
   enforcement ladder (Warning → Temporarily Limited Activities →
   Temporary Suspension → Permanent Ban) inspired by Mozilla's inclusion team.
2. **Adopter `[NOTE]` placeholder handling.** CV 3.0 ships with two
   bracketed editor notes that adopters are expected to resolve:
   - **Reporting an Issue** `[NOTE: describe your means of reporting here.]`
     — replaced with project-specific reporting details: email
     `meysam@shiehzadeh.de`; the "Community Moderators" role referenced
     throughout the document is filled by the project maintainer until a
     Technical Steering Committee is seated under `GOVERNANCE.md`
     (session 5; ≥3 maintainers triggers TSC formation per BUILD-PLAN);
     a PGP-encrypted disclosure channel for confidential reports follows
     in `SECURITY.md` (session 4).
   - **Addressing and Repairing Harm** `[NOTE: The remedies and repairs
     outlined below are suggestions… edit this section to describe your own
     policies.]` — removed without replacement. The canonical four-rung
     ladder is accepted as-is; we do not have a different established
     enforcement process to substitute.
3. **License interaction.** CV 3.0 is licensed under
   [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/),
   stewarded by the Organization for Ethical Source. Including the verbatim
   text as a separate `CODE_OF_CONDUCT.md` document does not change the
   project's `MIT OR Apache-2.0` source-code licence; the SA / ShareAlike
   obligation only triggers for derivative works of the CV text itself, and
   substituting documented adopter placeholders is normal adoption (the same
   posture taken by every major OSS project that ships CV). The canonical
   Attribution paragraph (CC BY-SA 4.0 link, FAQ / translations / resources
   pointers, Mozilla credit for the ladder) is preserved unmodified at the
   bottom of the file.
4. **No ADR planned by default.** CoC adoption is well-established,
   documented in BUILD-PLAN row 3, and not a controversial architectural
   decision. The maintainer may choose to record it alongside the
   session-7 ADR back-fills (after ADR-001 license, ADR-002 brand,
   ADR-003 DCO-over-CLA, ADR-004 in-repo-Action-over-Probot) if the
   format invites a "why CV verbatim, why these substitutions" entry.
   Otherwise the rationale stays in this log + BUILD-PLAN.

### Files

| Action | Path | Note |
|---|---|---|
| add | `CODE_OF_CONDUCT.md` | Contributor Covenant 3.0 verbatim from the canonical markdown release; two adopter `[NOTE]` placeholders substituted per decision 2; CC BY-SA 4.0 attribution preserved unmodified. Fetched via `curl -sSfL https://www.contributor-covenant.org/version/3/0/code_of_conduct/code_of_conduct.md`. |
| modify | `CONTRIBUTING.md` | § Code of Conduct flipped from placeholder ("lands as `CODE_OF_CONDUCT.md` in session 3 / be kind, be specific…") to a live link to `CODE_OF_CONDUCT.md` plus a pointer to the forthcoming PGP-encrypted disclosure channel in `SECURITY.md` (session 4). |
| modify | `CHANGELOG.md` | + Added: `CODE_OF_CONDUCT.md` entry with the substitution summary. + Decided: adopt CV 3.0 verbatim; canonical four-rung ladder accepted as-is. + Changed: `CONTRIBUTING.md` § Code of Conduct. |
| modify | `SESSION-LOG.md` | This entry. |

### Per-session gate sweep

| # | Gate | Status |
|---:|---|---|
| 11 | License + supply-chain green | ✅ No code dependencies. The single new doc dependency is CV 3.0 (CC BY-SA 4.0); compatible side-by-side with `MIT OR Apache-2.0` because distinct documents under distinct licences are independently distributable. Attribution preserved. |
| 13 | CHANGELOG.md updated | ✅ Added / Decided / Changed entries land alongside the file. |
| 16 | Glue-only honored | ✅ Canonical CV 3.0 text adopted verbatim; no parallel CoC reinvented. |
| 1, 2, 3, 4, 17 | Expert / reviewers / RFC / ADR / 48 h public review | ⚠️ Symbolic during bootstrap, same posture as sessions 1+2. RFC repo not built yet (session 6); ADR format not built yet (session 7); CoC adoption decision is forward-looking and may or may not become an ADR in session 7 (see decision 4). |
| 5–10, 12, 14–15, 18–19 | Test/perf/repro/SemVer/security/SBOM/random-repo | N/A — no Rust code, no CI matrix, no release, no fixture corpus at this stage. The DCO workflow remains untriggered until the first PR lands; this commit goes direct-to-`main` with `git commit -s` (signed off in the trailer). |

### Deferred follow-ups

- **Session 4 (next):** `SECURITY.md` ships a PGP key + dedicated
  disclosure channel + 90-day disclosure window + CVE coordination
  process + explicit RFSI alignment (Typomania, crate-quarantine RFC,
  provenance tracking). Once it lands, update **CODE_OF_CONDUCT.md
  § Reporting an Issue** ("A PGP-encrypted disclosure channel for
  confidential reports, and the formation of a designated Community
  Moderators group, will be added in upcoming governance sessions…")
  to refer to the live `SECURITY.md` PGP channel for confidential
  reports; the same forward-pointer in **CONTRIBUTING.md § Code of
  Conduct** ("a PGP-encrypted disclosure channel for confidential
  reports will follow in `SECURITY.md` (BUILD-PLAN session 4)") becomes
  a live link.
- **Session 5:** `GOVERNANCE.md` documents the TSC promotion path
  (≥3 maintainers triggers TSC formation). When a TSC is later seated,
  update **CODE_OF_CONDUCT.md § Reporting an Issue** to refer to the
  seated Community Moderators group rather than the single maintainer.
  No file edit needed in session 5 itself unless a TSC is seated then;
  the bootstrap-maintainer language is correct until that happens.
- **Session 6:** RFC repo `supergluepro/rfcs` lands. No CoC-side change
  needed; CONTRIBUTING.md already points at it as a forward reference.
- **Session 7:** ADR directory `/adr/` lands. Maintainer's choice on
  whether to record CoC adoption as an ADR; rationale already lives in
  this session log + BUILD-PLAN row 3 either way.
- **End of Phase A (when `supergluepro/superglue` flips public):**
  replicate `CODE_OF_CONDUCT.md` verbatim in that repo (same CC BY-SA 4.0
  attribution; same maintainer-fills-Community-Moderators-role
  language until a TSC is seated).
- **First live PR after `supergluepro/supergluepro` is on GitHub:**
  the DCO workflow runs end-to-end for the first time. Until then,
  DCO sign-off is a self-discipline (every commit on `main` since
  session 2 carries `Signed-off-by:`). This commit is the second
  direct-to-`main` `-s`-signed bootstrap commit; the SHA chain so far
  is `2e3c30a` (session 1, no sign-off — pre-DCO), `553d339` (session 2,
  no sign-off — added the workflow but pre-existed it on the same
  `main`-update batch), and this one (first sign-off-bearing commit on
  `main`).

### State for the next session

Tracked tree at the end of session 3:

```
.github/
  workflows/
    dco.yml          — DCO Signed-off-by enforcement on every PR
.gitignore           — standard + .claude/settings.local.json + user-text.txt excluded
BUILD-PLAN.md        — v10, "Resolved decisions" section
CHANGELOG.md         — Keep-a-Changelog 1.1.0; sessions 1+2+3 entries under [Unreleased]
CODE_OF_CONDUCT.md   — Contributor Covenant 3.0 verbatim (CC BY-SA 4.0); two adopter placeholders substituted
CONTRIBUTING.md      — DCO + Conventional Commits; § Code of Conduct now a live link
LICENSE-APACHE       — Apache 2.0 full text
LICENSE-MIT          — MIT
NOTICE               — minimal Apache-style notice
README.md            — License + Contributing pointer
SESSION-LOG.md       — sessions 1+2+3
```

Untracked / gitignored: `.claude/settings.local.json` and `user-text.txt`.

Session 4 (`SECURITY.md` + threat-model v0.1) is the next BUILD-PLAN row.
NCC / Cure53 / Trail of Bits-tier security expert. Deliverables:
PGP key generation (or reuse of an existing key on
`meysam@shiehzadeh.de`); 90-day responsible-disclosure window; CVE
coordination process; explicit alignment with Rust Foundation Security
Initiative outputs (Typomania typosquatting detection, crate-quarantine
RFC, provenance tracking). On landing, the "PGP-encrypted disclosure
channel will follow…" pointers in `CODE_OF_CONDUCT.md` § Reporting an
Issue and `CONTRIBUTING.md` § Code of Conduct become live links to
`SECURITY.md`.

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
