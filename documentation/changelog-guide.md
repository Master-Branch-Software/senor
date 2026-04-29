## CHANGELOG.md

A changelog is the upgrade-decision document. A user looking at version `2.7.0 → 2.8.0` opens it to answer one question: _what breaks if I upgrade._ Everything else — the marketing notes, the release blog post, the social announcement — is downstream. If the changelog cannot answer that question in under thirty seconds, the upgrade gets postponed and the team eventually upgrades through six versions of accumulated drift.

Changelogs are for humans, not machines. Do not paste `git log`. A commit documents a step in source-code evolution; a changelog entry communicates a noteworthy difference to the reader of the next release.

## File location and naming

- `CHANGELOG.md` at the repo root. Same level as `README.md` and `LICENSE`.
- One changelog per repo. A monorepo with independently versioned packages gets one `CHANGELOG.md` per package, located inside the package directory.
- Do not split into `CHANGELOG-2024.md`, `CHANGELOG-2025.md`. The file is read newest-first; older releases scroll off the bottom naturally.

## Two formats worth knowing

### Keep a Changelog (most common)

Six change-type headings, past-tense framing, an `Unreleased` section at the top:

- **Added** — new features.
- **Changed** — modifications to existing functionality.
- **Deprecated** — features marked for removal in a future release.
- **Removed** — features deleted in this release.
- **Fixed** — bug fixes.
- **Security** — vulnerability patches. Always called out, never folded into Fixed.

### Common Changelog (stricter alternative)

Four categories (`Changed`, `Added`, `Removed`, `Fixed`), present-tense imperative entries, no `Unreleased`, mandatory references (PR or commit) and authors per entry. Stricter, easier to automate, harder to draft by hand. Pick this when releases are batched and you can afford the extra discipline. Pick Keep a Changelog otherwise.

Pick one format and apply it. Mixing the two — past tense in some entries, imperative in others — reads as careless and makes diffs noisy.

## Required structure (Keep a Changelog)

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- New `--dry-run` flag for the migrate command.

## [2.8.0] - 2026-04-12

### Added

- IPv6 support for the proxy listener.

### Fixed

- Cache eviction loop on TTL=0 entries (#412).

### Security

- Bumped `lodash` to `4.17.21` to address CVE-2021-23337.

## [2.7.1] - 2026-03-30

### Fixed

- Crash on empty config file (#398).

[Unreleased]: https://github.com/owner/repo/compare/v2.8.0...HEAD
[2.8.0]: https://github.com/owner/repo/compare/v2.7.1...v2.8.0
[2.7.1]: https://github.com/owner/repo/compare/v2.7.0...v2.7.1
```

Required elements:

1. **Title and intro paragraph** stating the format and the versioning scheme.
2. **Unreleased section** at the top. Always present, even if empty between releases. New entries land here, then move under a version heading at release time.
3. **Version headings** newest-first, in the form `## [<version>] - <YYYY-MM-DD>`. ISO 8601 dates only — `04/12/26` is unreadable internationally.
4. **Change-type subheadings** in the canonical order (Added, Changed, Deprecated, Removed, Fixed, Security). Omit subheadings with no entries; do not write "None."
5. **Entries** as bullet points. One change per bullet. Reference the issue or PR number where it exists.
6. **Compare links** at the bottom. `[<version>]: <url>` with the GitHub/GitLab compare URL between adjacent tags. Allows the reader to click straight to the diff.

## Semver coupling

Keep a Changelog assumes Semantic Versioning. The two are bound:

- A `Removed` or breaking-`Changed` entry forces a MAJOR bump.
- An `Added` entry forces at least a MINOR bump.
- Only `Fixed` and `Security` (when non-breaking) entries permit a PATCH bump.

If the changelog and the version number disagree, the version is wrong. Do not ship a "patch" release with `Removed` entries. Either renumber the release or move the breaking change to the next minor.

`Deprecated` entries do not force a bump on their own — they are warnings. The eventual `Removed` entry is the breaking change.

## Voice

- One change per bullet. Compound bullets ("Fixed X and improved Y") hide one of the two changes from a `grep`.
- Lead with the user-visible effect, not the internal mechanism. "Faster startup on cold boot" beats "Refactored lazy-loader to defer import."
- Past tense for Keep a Changelog ("Added IPv6 support"); imperative for Common Changelog ("Add IPv6 support"). Pick one and stick with it.
- Reference the issue or PR. `(#412)` at the end of the bullet. Lets the reader open the discussion when one bullet is not enough.
- Do not editorialize. "Major performance improvements" without a number is marketing. Either cite the number ("30% faster on 10k-row imports") or drop the qualifier.
- Code voice for every flag, env var, function, and identifier.

## Generation strategies

| Strategy                         | When                                                              | Tool                                                                        |
| -------------------------------- | ----------------------------------------------------------------- | --------------------------------------------------------------------------- |
| Hand-written                     | Most projects. Default.                                           | —                                                                           |
| Conventional Commits → changelog | Project enforces commit format and ships small, frequent releases | `conventional-changelog`, `git-cliff`, `release-please`, `semantic-release` |
| Changesets                       | Monorepo with independent package versions                        | `changesets/changesets`                                                     |
| Notes drawn from PR titles       | GitHub-hosted, single-stream release                              | GitHub release-notes auto-generation                                        |

Automation saves work but does not substitute for review. Every generated changelog needs a human pass before release — to drop noise (`chore:` entries that affect no user), to rewrite cryptic PR titles into reader-facing prose, and to confirm the Security and Removed sections are accurate. A changelog generated and published without review is worse than no changelog, because the reader learns to distrust it.

## Release workflow

The mechanical steps, in order:

1. While developing, write entries under `## [Unreleased]` as part of the same PR that lands the change. Reviewers check the entry alongside the diff.
2. At release time, copy the `Unreleased` block to a new version heading, set the date, leave `Unreleased` empty.
3. Update the compare links at the bottom of the file. The new version's link compares the previous tag to the new one; the `Unreleased` link now compares the new tag to `HEAD`.
4. Tag the release commit. The tag and the heading version must match exactly.
5. Cut the GitHub/GitLab release, pasting the version's section as the release body. Do not write a parallel set of release notes — the changelog entry is the release note.

## Anti-patterns

- **Pasting `git log`.** The most common failure. A changelog generated mechanically from commits, with no human pass, includes merge commits, `wip` messages, internal refactors no user cares about, and revisions of revisions.
- **No dates, or ambiguous dates.** Without a date the reader cannot decide whether a release is recent. Use ISO 8601 (`YYYY-MM-DD`) without exception.
- **Generic entries.** "Bug fixes," "Performance improvements," "Various tweaks." A reader cannot upgrade from generic.
- **Missing `Security` entries.** A patched CVE without a `Security` entry is a trust failure. Even if the disclosure is embargoed, the entry lands at the embargo lift, not later.
- **Inconsistent voice.** Past tense in some entries, imperative in others. Pick a format.
- **Skipping versions.** Every released version gets a heading. A jump from `2.6.0` to `2.8.0` with no `2.7.x` entry signals either a missed release or a deleted history.
- **Inlining release-blog prose.** "We are thrilled to announce that..." Marketing belongs in the release blog post, not the changelog.
- **Marker comments instead of entries.** "Updated dependencies" with no list of which ones, at which versions. The reader who hits a behavior change cannot trace it.
- **Stale `Unreleased`.** `Unreleased` accumulates entries for six months and then ships as `v3.0.0`. If `Unreleased` is older than two months, either cut a release or move stale entries to a `Pending` section under explicit review.
- **AI fingerprints.** "Seamless upgrade experience," "robust new caching layer," "enhanced developer journey." Strip every word that would not survive a code review.

## Pre-release checklist

- [ ] Every entry under the new version corresponds to a real merged change.
- [ ] No entry is a duplicate of an entry already under a previous version.
- [ ] `Security` section is present if any vulnerability was patched.
- [ ] `Removed` and breaking-`Changed` entries match the version bump (MAJOR if present in a non-zero major).
- [ ] Every bullet names the user-visible effect, not the internal refactor.
- [ ] Every bullet that traces to an issue or PR has the reference appended.
- [ ] Date is ISO 8601.
- [ ] Compare links updated; the `Unreleased` link points to `HEAD`.
- [ ] Tag will match the version heading exactly.
- [ ] No `simply`, `seamless`, `robust`, `delve`, `leverage` in the prose.
- [ ] Generated entries (if any) have been read by a human and reworded where needed.

## Self-review

Before tagging, name one change you almost shipped without an entry, and one entry you wrote that is still cryptic to a reader who does not have your context. The first is a process bug; the second is a voice bug. Fix both before publishing.
