# Contributing to Superglue Pro

Thanks for considering a contribution. This document covers how to file issues
and submit changes to **this meta-repo** (`supergluepro/supergluepro`), which
holds the OSS roadmap, governance scaffolding, ADRs, and the session-by-session
build plan. Production code lives in
[`supergluepro/superglue`](https://github.com/supergluepro/superglue) (currently
private; flips public at the end of [Phase A](BUILD-PLAN.md#phase-a--foundation--early-runnability-sessions-117))
and will carry a separate `CONTRIBUTING.md` covering the Rust toolchain, build,
test, and run instructions. The DCO sign-off and Conventional Commit conventions
documented here apply org-wide.

## Where things live

| Repo | Purpose | Visibility |
|---|---|---|
| `supergluepro/supergluepro` (this) | Build plan, governance, ADRs, RFC index, session log | Public |
| [`supergluepro/superglue`](https://github.com/supergluepro/superglue) | Rust workspace, patterns, MCP server | Private until end of Phase A |
| `supergluepro/rfcs` | RFC discussions (Rust-language-team-style) | Public from session 6 |
| `supergluepro/patterns` | Pattern library | Public from Phase E |
| `supergluepro/fixture-corpus` | 50+ pinned random-open-repo fixtures + CI rail | Public from session 15 |
| `supergluepro/supergluepro.github.io` | Single-page redirect from `supergluepro.com` | Public |

## Reporting issues

Open issues at <https://github.com/supergluepro/supergluepro/issues>. For bugs,
include reproduction steps, expected vs. actual behaviour, and the relevant
session numbers from [BUILD-PLAN.md](BUILD-PLAN.md) if applicable. For ideas
that touch public surface, draft an RFC (see [RFCs](#rfcs)) instead of an
issue.

## Submitting changes

1. Fork and create a topic branch off `main`.
2. Make your change. Keep PRs small and focused.
3. Ensure every commit carries a [`Signed-off-by:` trailer](#dco-developer-certificate-of-origin).
4. Use [Conventional Commit](#conventional-commits) messages.
5. Open a PR against `main`. Reference any related issue or RFC.
6. Honour the [per-session 12/10 contract](BUILD-PLAN.md#per-session-1210-contract-every-session-honors-all-of-these)
   gates that apply to your change. The PR template (lands in session 8)
   mechanises this; until then, self-check against the list.
7. Allow the **48 h public review window** (gate 17) to elapse before merge for
   any change that touches public surface (BUILD-PLAN, README, governance docs,
   ADRs, RFCs). Symbolic during bootstrap (one contributor); becomes binding
   once a second maintainer is added.

## DCO (Developer Certificate of Origin)

Every commit must be signed off under the [Developer Certificate of Origin
1.1](https://developercertificate.org/). The sign-off attests that you wrote
the patch, or have the right to submit it under the project's licenses (`MIT
OR Apache-2.0`). It is **not** a Contributor License Agreement: there is no
copyright assignment, no separate document to sign, no record kept beyond the
sign-off trailer in `git log`.

### How to sign off

Use `-s` / `--signoff` on every commit:

```sh
git commit -s -m "feat: add foo"
```

This appends a trailer to your commit message:

```
Signed-off-by: Your Name <your.email@example.com>
```

The name and email must match the values configured in `git config user.name`
and `git config user.email` (and ideally match the email on your GitHub
account, so your commits link to your profile).

### Co-authored commits

Each author of a commit signs off independently. List co-authors with the
standard `Co-Authored-By:` trailer **and** include a `Signed-off-by:` line for
each:

```
Signed-off-by: Alice <alice@example.com>
Signed-off-by: Bob <bob@example.com>
Co-Authored-By: Bob <bob@example.com>
```

### If you forgot to sign off

For the most recent commit:

```sh
git commit --amend --signoff
git push --force-with-lease
```

For earlier commits in your branch:

```sh
git rebase --signoff main
git push --force-with-lease
```

### Enforcement

The [DCO check](.github/workflows/dco.yml) runs on every pull request. It
inspects each non-merge commit between the PR base and head and fails if any
commit is missing a `Signed-off-by:` trailer. Annotations on the PR identify
the offending commit SHAs.

### Why DCO and not a CLA

A CLA adds legal overhead (separate document, signing process, contributor
record-keeping) for marginal benefit on a permissive-OSS project. DCO is the
Linux-kernel norm and is sufficient for `MIT OR Apache-2.0`; the
Apache-2.0 §5-style inbound clause in [README.md](README.md#contribution) is
the actual licensing mechanism. (To be back-filled as ADR-003 in session 7.)

## Conventional Commits

Commit messages follow [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/).
From session 58 onward, `git-cliff` regenerates `CHANGELOG.md` directly from
these messages, so consistent prefixes matter:

```
<type>(<optional scope>): <subject>

<optional body>

<optional footers including Signed-off-by, Co-Authored-By, etc.>
```

Common types:

| Type | Use when |
|---|---|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `refactor` | Code change that neither adds a feature nor fixes a bug |
| `perf` | Performance improvement |
| `test` | Adding or correcting tests |
| `build` | Build system, dependency, or release tooling |
| `ci` | CI configuration |
| `chore` | Other housekeeping |

Use `!` after the type/scope to mark a breaking change (e.g. `feat(api)!:`).
Breaking changes also require an RFC for any release at or after `v1.0.0`.

For governance / planning sessions in this repo, the prefix `session-N:` is
also acceptable in addition to (not instead of) a Conventional type — this
matches the existing session-1 commit pattern.

## RFCs

Architectural changes require an RFC merged before code starts. RFCs live in
[`supergluepro/rfcs`](https://github.com/supergluepro/rfcs) once that repo is
created in session 6. Format: Rust-language-team RFC template; minimum 10-day
public comment window; two reviewer approvals.

## ADRs

Internal "why this and not that" decisions are recorded in `/adr/` in this
repo as Architectural Decision Records (Michael Nygard format). The ADR
directory is created in session 7 and back-fills the early decisions
(`ADR-001` license, `ADR-002` brand, `ADR-003` DCO over CLA, `ADR-004`
in-repo workflow over Probot DCO app, `ADR-005` MCP as v1.0 architecture,
`ADR-006` Claude Code as primary consumer).

## Code of Conduct

The Contributor Covenant 3.0 lands as `CODE_OF_CONDUCT.md` in session 3.
Until then: be kind, be specific, assume good faith. Conduct concerns can be
raised at <meysam@shiehzadeh.de>.

## License

By contributing you agree that your contribution is licensed under the
project's dual `MIT OR Apache-2.0` license, per the
[Contribution clause](README.md#contribution) in the README. No additional
agreement is required beyond the DCO sign-off.

## Communication

- **GitHub Issues** — bug reports, concrete tasks, tracked work.
- **GitHub Discussions** — open-ended questions, design conversations,
  community chatter. The only community surface until ≥1k stars (per
  BUILD-PLAN constraint: no Discord / Slack pre-traction).
- **`meysam@shiehzadeh.de`** — security disclosures (until session 4 ships
  `SECURITY.md` with PGP key + dedicated channel) and Code of Conduct
  concerns.
