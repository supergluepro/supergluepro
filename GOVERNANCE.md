# Governance

> **Status:** Pre-alpha, single-maintainer. The structures below are
> documented now so the project can grow into them without rewriting
> governance under load. Sections marked **(latent)** describe a state
> the project will enter once the relevant trigger fires (typically
> "≥3 active maintainers"). Until then, the single maintainer fills the
> role described.
>
> Changes to this document follow the [Updating this document](#updating-this-document)
> process at the bottom.

## Goals

Superglue Pro is a permissively-licensed (`MIT OR Apache-2.0`) open-source
CLI + MCP server that walks a codebase, scores files across seven signals,
and emits a structured context bundle for AI coding agents (Claude Code as
the canonical consumer; Cursor / Aider / Cline / Codex CLI / Goose as
first-class via MCP). Governance exists to make decision-making legible,
keep the [12/10 per-session quality bar](BUILD-PLAN.md#per-session-1210-contract-every-session-honors-all-of-these)
honoured as the contributor base grows, and document a clear path from
single-maintainer bootstrap to a multi-maintainer Technical Steering
Committee without ad-hoc improvisation when the project hits that
threshold.

The product roadmap lives in [`BUILD-PLAN.md`](BUILD-PLAN.md). This
document is the *process* layer: who decides what, how new contributors
move into trusted roles, what happens if the maintainer is hit by a bus,
and how this document itself gets updated.

## Roles

The role ladder follows the
[CNCF Project Template](https://github.com/cncf/project-template) /
[Minimum Viable Governance v1](https://github.com/aswf-track/MVG)
convention. Five rungs are defined now so future contributors can step
onto them naturally; the **Technical Steering Committee** rung is latent
and activates per the [TSC formation path](#technical-steering-committee-latent)
below.

| Rung | Privileges | How you get there |
|---|---|---|
| **User** | Files issues, opens discussions, downloads releases. | Use the project. |
| **Contributor** | Submits PRs, RFCs, pattern packs, fixture-corpus updates. | Open a PR. The first merged contribution makes you a contributor. |
| **Reviewer** | Provides substantive code review on incoming PRs. Reviews are weighted but do not carry merge rights. | Sustained, high-quality review activity over ≥3 months. Nominated by a maintainer; no objection from other maintainers within 14 days. |
| **Committer** | Direct merge rights on the relevant repo (e.g. `supergluepro/patterns`, `supergluepro/fixture-corpus`). Does not vote on project-wide direction. | Sustained, high-quality contribution activity over ≥6 months. Nominated by a maintainer; majority of existing maintainers approve within 14 days. |
| **Maintainer** | Project decision-making vote. Can nominate Reviewers / Committers / new Maintainers. Once ≥3 active maintainers exist, holds a TSC seat (see below). | Sustained, high-quality contribution and judgment over ≥12 months at the Committer rung (or equivalent). Nominated by a maintainer; supermajority (≥⅔) of existing maintainers approve within 30 days. The first additional maintainer beyond the founding maintainer is the most consequential — see [Adding the first additional maintainer](#adding-the-first-additional-maintainer). |
| **Technical Steering Committee (TSC)** **(latent)** | Project-wide architectural and strategic decisions; tie-breaker votes; arbitration of disputes that cannot be resolved at the maintainer level. | Activates per the [TSC formation path](#technical-steering-committee-latent). |

### Current roster

- **Maintainer:** Dipl.-Ing.(FH) Meysam Shiehzadeh — `meysam@shiehzadeh.de` — GitHub: project owner of `supergluepro`.

There are no other Reviewers, Committers, or Maintainers at this time.
This roster moves to a separate `MAINTAINERS` file once a second
maintainer is onboarded; until then, it lives here for simplicity.

### Adding the first additional maintainer

The transition from one maintainer to two is the most consequential in
this project's life — it is the point at which the 48-hour public-review
gate (gate 17) becomes substantively binding rather than symbolic, and
the point at which decision-making moves from "the founding maintainer
chooses" to "two people must agree." The founding maintainer commits to:

1. Publishing the nomination as an RFC against `supergluepro/rfcs` (once
   that repo is created in [BUILD-PLAN session 6](BUILD-PLAN.md#phase-a--foundation--early-runnability-sessions-117))
   with a 30-day public comment window — twice the standard 14-day window
   for subsequent maintainer additions.
2. Stating the candidate's contribution history, the rationale, and the
   transition plan (which repos receive merge rights immediately, which
   require a probation period).
3. Honouring the comment window even if no objections surface, so that
   the absence of objection is documented rather than assumed.

Subsequent maintainer additions follow the standard 14-day window and
supermajority approval described in the role table.

### Removing roles

Roles are removed for one of three reasons:

1. **Resignation.** Any role-holder may step down at any time by opening
   an issue or notifying the maintainer privately. Outgoing maintainers
   are encouraged but not required to nominate a successor.
2. **Inactivity.** If a Reviewer / Committer / Maintainer has had no
   substantive activity (PRs, reviews, RFCs, security responses) for
   ≥6 months, any maintainer may propose moving them to **emeritus**
   status by RFC. Emeritus role-holders retain credit but lose merge
   rights and TSC voting; they may return to active status at any time
   without re-nomination if their inactivity was ≤24 months.
3. **Code of Conduct violation.** Per the
   [Code of Conduct enforcement ladder](CODE_OF_CONDUCT.md#addressing-and-repairing-harm).
   At rungs 3 (Temporary Suspension) and 4 (Permanent Ban), any merge
   rights, TSC seat, and nomination privileges are revoked for the
   duration of the suspension or permanently, respectively.

## Decision making

Decisions are categorised by scope; each category has its own process.
The 19-gate per-session contract in [`BUILD-PLAN.md`](BUILD-PLAN.md#per-session-1210-contract-every-session-honors-all-of-these)
applies on top of all of these — gates are the floor, not the ceiling.

### Routine changes (PRs that do not touch public surface)

Default mechanism: **lazy consensus**. A PR with at least one Reviewer
or Maintainer approval and no maintainer objection within 5 business
days may be merged.

While the project has a single maintainer, "lazy consensus" reduces to
"the maintainer reviews and merges." Once a second maintainer is added,
the 5-day comment window becomes substantive.

### Architectural changes

Any change affecting cross-cutting design (signal model, output schema,
MCP tool surface, fixture-corpus structure, build pipeline) requires:

1. An RFC merged in [`supergluepro/rfcs`](https://github.com/supergluepro/rfcs)
   (once that repo is created in BUILD-PLAN session 6).
2. **Minimum 10-day public comment window** on the RFC.
3. **At least 2 reviewer approvals**, at least one from a different
   specialty than the RFC author.
4. Once approved, an ADR landed in `/adr/` (created in BUILD-PLAN session 7)
   recording the resolution before code changes start.

This is the project-wide expression of BUILD-PLAN sequencing rule 6:
*every architectural decision is recorded as an ADR before code changes;
every public-surface decision as an RFC before merge.*

### Public-surface changes

Changes to `BUILD-PLAN.md`, `README.md`, governance docs, ADRs, RFCs, the
output-schema contract (frozen in BUILD-PLAN session 13), or the MCP tool
schemas (frozen in Phase C) require the **48-hour public review window**
on the PR (gate 17). The window starts when the PR is marked ready for
review.

While the project has a single maintainer, gate 17 is honoured as a
self-discipline (the maintainer waits 48 h before merging public-surface
PRs even though no second reviewer exists). It becomes substantively
binding once a second maintainer is added.

### Governance changes (changes to this document)

See [Updating this document](#updating-this-document) at the bottom.
Higher bar than other public-surface changes: RFC + 30-day window +
(once seated) TSC supermajority.

### Code of Conduct enforcement

Per the [enforcement ladder in `CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md#addressing-and-repairing-harm).
While the project has a single maintainer, the **Community Moderators**
role referenced throughout the Code of Conduct is filled by the
maintainer. Once a TSC is seated, the TSC takes over the Community
Moderators role; alternatively, the TSC may delegate to a separate
Conduct Committee by RFC.

### Security disclosure handling

Per [`SECURITY.md`](SECURITY.md). 90-day coordinated-disclosure window;
GitHub Private Vulnerability Reporting primary; CVSS v4.0; CVE
coordination via GitHub as a CNA. The single maintainer triages until a
TSC + Security Response Group are seated; the seated group is documented
in `SECURITY.md` § Hall of fame neighbouring section when the time comes.

## Technical Steering Committee (latent)

A Technical Steering Committee activates when the project sustains
**≥3 active maintainers** (active = at least one substantive contribution
or review in the prior 6 months). The threshold is set by
[BUILD-PLAN row 5](BUILD-PLAN.md#phase-a--foundation--early-runnability-sessions-117);
the active-window form is the CNCF norm.

Until that threshold is reached, every reference below is forward-looking.

### Trigger and formation

When the third active maintainer is confirmed, any of the existing
maintainers opens an RFC against `supergluepro/rfcs` to formally seat
the TSC. The RFC:

1. Names the initial TSC members (default: the active maintainers at
   that time, all seated; if there are more than 5, the maintainers
   nominate 5 by mutual agreement).
2. Sets the term length and stagger schedule.
3. Sets the election cadence for subsequent terms.
4. Sets quorum and supermajority thresholds.
5. Sets the meeting cadence (minimum: monthly; recorded; transcript
   posted to GitHub Discussions).

### Default composition

- **5 seats**, odd-sized to avoid 2-2 deadlocks. Adjustable upward to 7
  by TSC supermajority once the project has ≥7 active maintainers.
- **1-year terms, staggered** (3 seats expire one year, 2 seats the next)
  so institutional memory survives turnover.
- **No more than ⅔ of seats from a single employer** to keep
  decision-making independent of any single sponsor.
- **Re-election allowed** without term limit at this stage; revisitable
  by RFC if and when single-employer concentration becomes a concern.

### Responsibilities

The TSC:

1. Resolves disputes among maintainers that cannot be resolved by lazy
   consensus or supermajority maintainer vote.
2. Approves RFCs that significantly affect public surface (output schema,
   MCP tool surface, distribution paths, license, brand) when the RFC
   author and reviewers cannot reach consensus.
3. Approves the project's annual security-audit vendor (BUILD-PLAN
   sessions 36, 139).
4. Approves CNCF Sandbox / Incubation / Graduation submissions if and
   when the project pursues those paths (BUILD-PLAN Phase I).
5. Owns the `GOVERNANCE.md` document — see
   [Updating this document](#updating-this-document).

The TSC does **not** review every PR; routine and most architectural
decisions remain at the maintainer level. The TSC is the body of last
resort, not a permission gate on day-to-day work.

### Voting

- **Quorum:** majority of seats present.
- **Routine motions:** simple majority.
- **Governance changes, security-policy changes, license changes,
  brand changes, CNCF-track decisions, vendor selections:** supermajority
  (≥⅔ of all seats, not just those present).
- **Recusal:** any TSC member with a conflict of interest (see below)
  abstains from the relevant vote. A recused vote does not count
  toward quorum or supermajority denominators.

## Succession and continuity

Single-maintainer projects carry an irreducible **bus factor of one**.
This section documents the practical continuity plan until that bus
factor is reduced by additional maintainers.

### If the maintainer becomes unavailable

1. **Domain.** `supergluepro.com` is registered to Dipl.-Ing.(FH) Meysam
   Shiehzadeh via [united-domains GmbH](https://www.united-domains.de/) (DE),
   registered 2026-04-15, expiring 2027-04-15. Domain transfer follows
   the registrar's standard procedures (auth code retrieval, transfer
   to a successor's chosen registrar). Next-of-kin or estate executor
   should contact united-domains directly with proof of authority.
2. **GitHub organisation.** The `supergluepro` GitHub organisation has
   the maintainer as sole owner. GitHub's organisation-transfer process
   for incapacitated or deceased account holders is documented at
   [GitHub Docs — "Reasoning about ownership"](https://docs.github.com/en/site-policy/other-site-policies/github-deceased-user-policy).
   Successors should follow GitHub's published procedure rather than
   attempting to recover credentials.
3. **Forking.** The dual `MIT OR Apache-2.0` license means any contributor
   or downstream user may fork and continue development independently
   without permission. A fork is the project's safety net — if the
   maintainer is permanently unavailable and no successor is appointed,
   the community may continue under any name they choose.
4. **Pre-`v0.1.0` artefacts.** No release-signing keys exist yet. Once
   release signing lands (BUILD-PLAN session 25, Sigstore-keyless OIDC),
   the keyless flow means no single maintainer holds long-lived signing
   secrets — successors do not need to recover the prior maintainer's
   keys to continue signing.

### Reducing bus-factor over time

The single most effective continuity mechanism is **adding a second
maintainer**. The procedure for the first additional maintainer is
documented in [Adding the first additional maintainer](#adding-the-first-additional-maintainer)
above. The founding maintainer commits to actively recruiting at the
[`v0.1.0`](BUILD-PLAN.md#phase-b--quality-hardening-sessions-1838) and
[`v1.0.0`](BUILD-PLAN.md#phase-d--distribution-sessions-4961) milestones.

Until that succeeds, the maintainer maintains:

- Public, written documentation of every architectural decision (ADRs +
  RFCs).
- Public session log (`SESSION-LOG.md`) so a fresh contributor can pick
  up cold.
- All credentials kept off-repo and in a personal password manager;
  no single point of digital-credential failure beyond GitHub auth and
  the domain registrar.

## Conflict of interest

Maintainers, Committers, and (once seated) TSC members disclose:

1. **Employer or primary contracting relationship.** Updated within
   30 days of any change.
2. **Substantial financial relationship with a downstream consumer of
   Superglue Pro** (e.g. equity, advisory role, sponsored work) where
   "substantial" is left to the discloser's good-faith judgment but
   defaults to >USD 1,000 / month or >0.1% equity.
3. **Authorship or maintenance of a directly-competing tool**
   (Repomix, gitingest, Aider repo-map, similar code-context
   extractors).

Disclosures live in this file's [Current roster](#current-roster) section
(or in `MAINTAINERS` once that file exists). They are public.

### Recusal

A role-holder with a conflict of interest:

- **Recuses from votes** on RFCs or motions where the conflict applies.
- **Discloses up front** in PR / RFC / issue comments where the conflict
  applies.
- **Does not nominate** candidates with whom they share a disclosed
  conflict (e.g. an employer reviewer cannot nominate a colleague at the
  same employer to maintainership without a second nominator from a
  different employer).

The bar is "would a reasonable observer expect this person to favour
their conflicted interest over the project's." When in doubt, disclose
and recuse.

## Code of Conduct

This project adopts the [Contributor Covenant 3.0](CODE_OF_CONDUCT.md)
verbatim. The "Community Moderators" role referenced throughout is
filled by the single maintainer until a TSC is seated; the TSC then
inherits the role (or delegates to a Conduct Committee by RFC).

Reporting and confidential disclosure: see
[`CODE_OF_CONDUCT.md` § Reporting an Issue](CODE_OF_CONDUCT.md#reporting-an-issue)
and [`SECURITY.md`](SECURITY.md). The enforcement ladder is the
canonical four-rung CV 3.0 ladder (Warning → Temporarily Limited
Activities → Temporary Suspension → Permanent Ban); modifications
require a governance-document RFC per
[Updating this document](#updating-this-document).

## Trademark and brand

Per [BUILD-PLAN § Resolved decisions](BUILD-PLAN.md#1-trademark--naming--stay-generic-brand-anchored-by-domain--github-org),
the project does not pursue a registered trademark at this stage.
The "Superglue Pro" brand is anchored by:

- The `supergluepro.com` domain (Dipl.-Ing.(FH) Meysam Shiehzadeh,
  united-domains GmbH).
- The `supergluepro` GitHub organisation.
- Consistent use of "Superglue Pro" as the project display name in
  `LICENSE-MIT`, `LICENSE-APACHE`, `NOTICE`, `BUILD-PLAN.md`, README,
  CHANGELOG, future MCP tool / package metadata, and any future
  release artefacts.

Forks are welcome under the dual MIT/Apache license but must not use
"Superglue Pro" as their primary brand name to avoid downstream
confusion. This is a request rooted in good-faith branding norms, not
a legal restriction; the dual license itself imposes no naming
constraint.

## Updating this document

Changes to this document are governance changes and have a higher bar
than other public-surface changes:

1. **RFC required.** Open an RFC against `supergluepro/rfcs` describing
   the proposed change and the rationale.
2. **30-day public comment window.** Three times the routine 10-day RFC
   window — governance changes deserve more time to surface concerns.
3. **Approval threshold:**
   - **Until the TSC is seated:** all current maintainers must approve.
     While the project has a single maintainer, that reduces to
     "maintainer approves," but the 30-day window is honoured to
     document the absence of objection rather than assume it.
   - **Once the TSC is seated:** TSC supermajority (≥⅔ of all seats,
     not just those present), with recusals not counted toward the
     denominator.
4. **Versioning.** This document is versioned by reference to the
   BUILD-PLAN session that produced or last modified it. The current
   version is the [BUILD-PLAN session 5](BUILD-PLAN.md#phase-a--foundation--early-runnability-sessions-117)
   initial draft. Subsequent revisions update the *Last updated* line
   at the bottom and add a one-line entry to `CHANGELOG.md` § Changed.

Emergency exceptions: the maintainer (or, once seated, the TSC) may
adopt a security- or legal-emergency change with a shortened window
(minimum 5 days, with public reasoning attached to the RFC). The
emergency change is ratified or reverted at the next regular governance
review.

## Attribution

This document adapts the
[CNCF Project Template](https://github.com/cncf/project-template) and
the [Minimum Viable Governance v1](https://github.com/aswf-track/MVG)
guide. Both are licensed under permissive terms (Apache-2.0 / CC-BY-4.0)
and explicitly intended for downstream adoption with project-specific
customisation. The Code of Conduct ladder referenced from this document
is the [Contributor Covenant 3.0](https://www.contributor-covenant.org/version/3/0/)
ladder (CC BY-SA 4.0; full attribution in `CODE_OF_CONDUCT.md`).

The role-ladder structure (User → Contributor → Reviewer → Committer →
Maintainer → TSC) is the CNCF-norm five-plus-one ladder used by
Kubernetes, Envoy, Prometheus, OpenTelemetry, Sigstore, and similar
projects.

---

**Last updated:** 2026-04-26 (BUILD-PLAN session 5).
**Maintainer:** Dipl.-Ing.(FH) Meysam Shiehzadeh — `meysam@shiehzadeh.de`.
**Reporting a Code of Conduct concern:** see [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md#reporting-an-issue).
**Reporting a security vulnerability:** see [`SECURITY.md`](SECURITY.md).
