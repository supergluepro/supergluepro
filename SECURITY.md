# Security policy

> **Status:** Pre-alpha — no public release yet. The policy below applies to
> all code, documentation, and infrastructure under the `supergluepro/*`
> GitHub organization. The first tagged artefact will be `v0.0.1-alpha` at
> the end of [Phase A](BUILD-PLAN.md#phase-a--foundation--early-runnability-sessions-117)
> (BUILD-PLAN session 14).

## Reporting a vulnerability

We accept and encourage **coordinated, private** disclosure of security
issues in any `supergluepro/*` repository, release artefact, or published
pattern. Please use the channels below in order of preference.

### 1. Preferred — GitHub Private Vulnerability Reporting (GPVR)

Open a private security advisory at:

> **<https://github.com/supergluepro/supergluepro/security/advisories/new>**

GPVR is the canonical 2026 channel for OSS vulnerability disclosure on
GitHub. It keeps the report on the same surface as the code, lets us track
triage / fix / advisory / CVE in one place, and notifies maintainers
automatically. Use it as the first option.

When the report concerns a different `supergluepro/*` repository (for
example issues in the production code at `supergluepro/superglue` once it
ships, or in the patterns at `supergluepro/patterns`), open the GPVR
advisory in that repository instead. The disclosure policy in this document
applies org-wide.

### 2. Email backup

If GPVR is not available to you (your account cannot file private
advisories, or the report concerns the GitHub account / org itself), email:

> **`meysam@shiehzadeh.de`**

A `security@supergluepro.com` forwarder will be provisioned once the
domain's DNS is moved off the united-domains parking nameservers
(BUILD-PLAN session 17). Until then the maintainer's email above is the
only email channel.

### 3. PGP — end-to-end encryption over email

For end-to-end-encrypted reports over email:

```
Fingerprint:  TBD — to be published as a session-4 deferred follow-up.
Public key:   not yet published.
Keyserver:    keys.openpgp.org (planned)
GitHub GPG:   github.com/<maintainer-handle>.gpg (planned)
```

A PGP key on `meysam@shiehzadeh.de` will be published once generated on the
maintainer's hardware. Until then, reports requiring end-to-end encryption
should use **GPVR** (encrypted in transit and at rest by GitHub).

## What to include in your report

- A clear description of the issue and its impact.
- Steps to reproduce — minimal repository, command, or input that triggers
  the issue.
- The affected version, commit SHA, or release artefact (where applicable).
- Your assessment of severity and any suggested mitigations.
- Whether you intend to publish independently and on what timeline, so we
  can coordinate.

We will acknowledge receipt within **3 business days** and provide an
initial triage assessment within **10 business days**.

## Disclosure timeline

We follow a **90-day coordinated-disclosure window**, consistent with the
Project Zero norm and with CISA's Coordinated Vulnerability Disclosure
guidance. The clock starts on receipt of a complete report.

| Day | Milestone |
|---:|---|
| 0 | Report received; acknowledged within 3 business days. |
| 0–10 | Triage; severity scored (CVSS v4.0); fix scoped. |
| 10–60 | Fix developed, reviewed, tested. |
| ~60 | Patched release prepared; CVE ID requested via the GitHub Security Advisory workflow (GitHub is a CVE Numbering Authority). |
| ~75 | Reporter notified of release timing; pre-disclosure to known downstream consumers if warranted. |
| ≤90 | Public release + advisory + CVE published. |

We may request an extension if the fix is large or coordinated across
multiple ecosystems. We will publish on day 90 even if the fix is
incomplete when continued silence creates more risk than disclosure.

## Severity scoring

Severity is scored using **[CVSS v4.0](https://www.first.org/cvss/v4-0/)**
(FIRST, November 2023), the current industry standard. The advisory
carries both the v4.0 vector and the qualitative severity (None / Low /
Medium / High / Critical). Where a consuming ecosystem requires it, we
also publish the v3.1 vector.

## CVE coordination

GitHub is a CVE Numbering Authority (CNA) and assigns CVE IDs for
vulnerabilities in projects hosted on `github.com`. CVE IDs for accepted
reports are requested through the GitHub Security Advisory workflow during
fix development, and published alongside the public advisory at release.

## Supported versions

Until the first release tag (`v0.0.1-alpha`, end of Phase A — BUILD-PLAN
session 14), security fixes target `main` only.

Once `v0.0.1-alpha` ships, security fixes target the **current minor and
the immediately previous minor** at minimum (e.g. fixes to `v0.3.x` are
also backported to `v0.2.x`). After `v1.0.0` (end of Phase D — BUILD-PLAN
session 61) the supported matrix may be widened; the matrix as of any
given release is documented in the release's `CHANGELOG.md` entry.

## Scope

**In scope:**

- Source code in any `supergluepro/*` repository.
- Build, release, and CI infrastructure (GitHub Actions workflows, release
  pipelines, OIDC-token publishing flows once they ship in BUILD-PLAN
  session 25).
- Pattern library content in `supergluepro/patterns` (once Phase E ships).
- Fixture corpus content in `supergluepro/fixture-corpus` (once
  BUILD-PLAN session 15 ships).
- Distribution packages published by `supergluepro/*` (Homebrew, Scoop,
  Winget, AUR, Nix, Docker, apt, rpm, crates.io — Phase D).

**Out of scope:**

- Vulnerabilities in third-party dependencies — please report those
  upstream first. We will accept reports on dependency-choice issues
  (e.g. a vulnerable pinned version we should bump, or a typosquatted
  dependency that should be replaced).
- Vulnerabilities in the unrelated `superglue.com` product (different
  project, different category — see [`supergluepro.com`](https://supergluepro.com)
  vs. [`superglue.com`](https://superglue.com)).
- Issues in repositories that fork or vendor `supergluepro/*` code —
  please contact the fork maintainers.
- Claims requiring physical access to a developer's machine, root-level
  privilege escalation on a system already compromised, or social
  engineering of contributors.

## Safe harbor

Good-faith security research on `supergluepro/*` projects is welcomed. We
will not pursue legal action against researchers who:

- Report through the channels above.
- Avoid privacy violations, destruction of data, and disruption of services.
- Honor a reasonable disclosure timeline.
- Do not publish details before coordinated disclosure.

We publicly credit reporters in advisories unless they request otherwise.

## Supply-chain security alignment

Superglue Pro aligns with the **Rust Foundation Security Initiative (RFSI)**
outputs as they ship:

- **Typomania** — typosquatting detection for crates.io publishing flows.
- **Crate-quarantine RFC** — quarantined publishing flow for newly-uploaded
  crates pending automated and human review.
- **Provenance tracking** — cargo + Sigstore + crates.io provenance metadata.

These are tracked as ambient discipline in BUILD-PLAN [Phase I](BUILD-PLAN.md#phase-i--ongoing-rails-continuous).
When a new RFSI output ships, we adopt it. When adoption changes our
disclosure surface or threat model, we update this document and
[`THREAT-MODEL.md`](THREAT-MODEL.md).

Future supply-chain controls already scheduled in BUILD-PLAN:

| Session | Control |
|---:|---|
| 18 | Branch protection + signed commits via Sigstore `gitsign` (keyless OIDC). |
| 20 | `cargo audit` + `cargo deny` + `cargo vet` + Renovate dep updates in CI. |
| 21 | CodeQL static analysis (Rust GA) on PR + nightly. |
| 23 | `cargo fuzz` with 6 fuzz targets (pattern loader, YAML parser, ast-grep input, walker, completeness, output renderer). |
| 24 | SLSA L3 build attestations + CycloneDX 1.6 SBOM per release. |
| 25 | Sigstore-signed releases + crates.io Trusted Publishing (RFC 3691, OIDC; long-lived API tokens disabled). |
| 26 | Reproducible builds (`trim-paths`, `SOURCE_DATE_EPOCH`, `--locked`). |
| 32 | OpenSSF Scorecard hardening to ≥9.5; SHA-pinned third-party Actions; `SECURITY-INSIGHTS.yml` published. |
| 36 | Annual external security audit pre-engagement (NCC / Trail of Bits / Cure53). |
| 139 | First annual external audit completed; report published in repo. |

## Threat model

A v0.1 threat model lives at [`THREAT-MODEL.md`](THREAT-MODEL.md). It is
revised whenever a new architectural surface lands; major version bumps
coincide with BUILD-PLAN phase milestones.

## Hall of fame

Reporters who responsibly disclosed accepted vulnerabilities are credited
here. *(No entries yet — Superglue Pro is pre-alpha.)*

---

**Last updated:** 2026-04-26 (BUILD-PLAN session 4).
**Document version:** v0.1.
