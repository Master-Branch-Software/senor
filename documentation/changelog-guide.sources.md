# Sources — changelog-guide.md

Citations backing claims in [`changelog-guide.md`](changelog-guide.md). Developer artifact — not loaded by skill consumers. Update in the same commit when chapter rules change.

Per `CONTRIBUTING.md`:

- Human-authored sources only (specs, established engineering blogs, production codebases, books). No AI-generated content.
- Cite the specific claim, not just the topic.
- Mark uncited rules inline ("uncited — verify before publishing") rather than fabricating sources.

## Canonical references

- [Keep a Changelog 1.1.0 — Olivier Lacan](https://keepachangelog.com/en/1.1.0/) — the six change-type categories (Added, Changed, Deprecated, Removed, Fixed, Security), the seven guiding principles ("for humans, not machines," every version gets an entry, group identical types, link versions, latest first, display dates, semver adherence), the `Unreleased` section convention, ISO 8601 dates, and the rule against pasting `git log`.
- [Common Changelog](https://common-changelog.org/) — alternative format with stricter rules: present-tense imperative entries, four categories (Changed, Added, Removed, Fixed), no `Unreleased` section, mandatory references and authors per entry. Source of the imperative-vs-past-tense contrast in the chapter.
- [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html) — exact MAJOR/MINOR/PATCH definitions, the rule that breaking changes force MAJOR, that backwards-compatible features force MINOR, and that backwards-compatible fixes force PATCH. Backs the "semver coupling" section.
- [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) — defines the commit-format-to-changelog automation path. Cited under "Generation strategies."
- [GitHub: Automatically generated release notes](https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes) — backs the "PR-title generation" entry in the strategy table.

## Tool catalog

- [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog) — generates a changelog from Conventional Commits history. Reference implementation of the spec-to-changelog pipeline.
- [git-cliff](https://git-cliff.org/) — Rust-based generator with templating, customization, and monorepo support. Often a better default than `conventional-changelog` for non-Node projects.
- [release-please](https://github.com/googleapis/release-please) — Google's release-PR automation; opens a PR that updates the changelog and version, merges to release. Cited as the GitHub-Action-friendly option.
- [semantic-release](https://semantic-release.gitbook.io/semantic-release/) — fully automated semver and changelog from Conventional Commits. Cited as the "no human in the loop" extreme — chapter calls out the cost.
- [changesets](https://github.com/changesets/changesets) — monorepo-aware, contributor-authored change notes that aggregate at release time. Cited under "Monorepo with independent package versions."
- [github-changelog-generator](https://github.com/github-changelog-generator/github-changelog-generator) — fills changelogs from closed issues and merged PRs. Older, still in use.

## Curated examples

Production changelogs worth reading. Each is cited because it gets one specific aspect right.

- [Vue.js CHANGELOG](https://github.com/vuejs/core/blob/main/CHANGELOG.md) — exemplary Conventional-Commits-derived changelog with consistent voice and clean per-version structure.
- [Rails CHANGELOG (per-component)](https://github.com/rails/rails/blob/main/activerecord/CHANGELOG.md) — multiple changelogs in a monorepo, hand-curated, model for "user-visible effect first."
- [Rust release notes](https://github.com/rust-lang/rust/blob/master/RELEASES.md) — narrative-style release notes that complement (do not replace) the granular changelog.
- [Babel CHANGELOG.md](https://github.com/babel/babel/blob/main/CHANGELOG.md) — automated via Lerna/changesets; demonstrates the "generated then reviewed" workflow.
- [keep-a-changelog's own CHANGELOG](https://github.com/olivierlacan/keep-a-changelog/blob/main/CHANGELOG.md) — meta-example, dogfoods every rule in the spec.

## Uncited / heuristic claims to verify

- The "Unreleased older than two months" trigger is editorial. No source sets a number; the heuristic is to cap drift before the file becomes a release-notes graveyard.
- The recommendation to one-changelog-per-package in monorepos derives from observed practice in `changesets`-using monorepos (see Babel, Vue, Astro).
- The "every generated changelog needs a human pass" claim is an editorial position consistent with Keep a Changelog's "for humans" principle. Source does not state the rule explicitly.
- The "thirty-second upgrade decision" framing is editorial.

## Cross-references

- Commit-format integration → [`contributing-guide.md`](contributing-guide.md) § Commit conventions.
- Security entries and disclosure timing → [`repo-meta-files.md`](repo-meta-files.md) § SECURITY.md.
- Voice and AI-fingerprint vocabulary → `front-end/brand-and-copywriting.md`.
- README's role vs changelog's role → [`readme-guide.md`](readme-guide.md) § Length and scope ("A `CHANGELOG.md` belongs next to the README, not inside it.").
