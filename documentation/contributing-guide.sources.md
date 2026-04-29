# Sources — contributing-guide.md

Citations backing claims in [`contributing-guide.md`](contributing-guide.md). Developer artifact — not loaded by skill consumers. Update in the same commit when chapter rules change.

Per `CONTRIBUTING.md`:

- Human-authored sources only (specs, established engineering blogs, production codebases, books). No AI-generated content.
- Cite the specific claim, not just the topic.
- Mark uncited rules inline ("uncited — verify before publishing") rather than fabricating sources.

## Canonical references

- [GitHub: Setting guidelines for repository contributors](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/setting-guidelines-for-repository-contributors) — the canonical list of locations GitHub recognizes for `CONTRIBUTING.md` (`./`, `.github/`, `docs/`) and the surfacing behavior on issue/PR pages.
- [Mozilla Science Lab — Wrangling Web Contributions: How to Build a CONTRIBUTING.md](https://mozillascience.github.io/working-open-workshop/contributing/) — source of the canonical section list (welcome, table of contents, important resources, testing, environment, submission protocol, bug reports, first bugs, enhancement requests, style guide, code of conduct, recognition, contact). Underpins the "Required sections (in order)" list in the chapter.
- [contributing.md — How to Build a CONTRIBUTING.md](https://contributing.md/how-to-build-contributing-md/) — backs the "scope of contributions up front" rule and the "ways to contribute beyond code" guidance.
- [Open Source Guides — Starting an Open Source Project (GitHub)](https://opensource.guide/starting-a-project/) — backs the recommendation to point security reports to `SECURITY.md`, the SLA-honesty principle, and the recognition-model framing.
- [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) — exact header format, allowed types (`feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`), breaking-change syntax (`!` and `BREAKING CHANGE:` footer), and the semver mapping (`fix` → PATCH, `feat` → MINOR, breaking → MAJOR).
- [Angular contribution guide](https://github.com/angular/angular/blob/main/CONTRIBUTING.md) — origin of the Conventional Commits style; example of a large project's living CONTRIBUTING file.
- [Developer Certificate of Origin 1.1](https://developercertificate.org/) — exact text the DCO requires; backs "DCO via `git commit -s`" claim.
- [Linux Foundation — DCO vs CLA](https://www.linuxfoundation.org/blog/blog/cla-vs-dco-whats-the-difference) — backs the chapter's framing that the project must pick one (DCO, CLA, neither) and explain it in one sentence.
- [GitHub: About issue and pull request templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/about-issue-and-pull-request-templates) — the `.github/ISSUE_TEMPLATE/` requirement and `.github/PULL_REQUEST_TEMPLATE.md` location.
- [The Good Docs Project — CONTRIBUTING template](https://gitlab.com/tgdp/templates/-/blob/main/CONTRIBUTING.md) — corroborates the section list and ordering for documentation-heavy projects.

## Curated examples

Production CONTRIBUTING files worth reading before drafting one. Each is cited because it gets one specific aspect right, not because the whole file is exemplary.

- [rust-lang/rust](https://github.com/rust-lang/rust/blob/master/CONTRIBUTING.md) — minimal root file pointing into a much deeper `rustc-dev-guide`. Good model for the "split when long" rule.
- [kubernetes/kubernetes](https://github.com/kubernetes/community/blob/master/contributors/guide/README.md) — exemplifies the "ways to contribute beyond code" section (SIGs, triage, docs, translations).
- [home-assistant/core](https://github.com/home-assistant/core/blob/dev/.github/CONTRIBUTING.md) — concise, links out to a full developer site; demonstrates `.github/` placement.
- [angular/angular](https://github.com/angular/angular/blob/main/CONTRIBUTING.md) — dense Conventional Commits documentation with an example block.
- [PurpleBooth/Good-CONTRIBUTING.md-template](https://gist.github.com/PurpleBooth/b24679402957c63ec426) — widely forked starter template; cited for ordering conventions.

## Tool catalog

- [commitlint](https://commitlint.js.org/) — enforces Conventional Commits via Husky pre-commit hook. Cited where the chapter recommends naming the convention with an example.
- [DCO GitHub App](https://github.com/apps/dco) — enforces the DCO sign-off in PRs. Cited for the sign-off section.
- [CLA Assistant](https://github.com/cla-assistant/cla-assistant) — automates CLA signature collection. Cited for the CLA option.
- [all-contributors](https://allcontributors.org/) — recognition automation. Cited for the recognition section.
- [act](https://github.com/nektos/act) — runs GitHub Actions locally. Cited indirectly under "setup that runs on a clean checkout" — useful for verifying CI parity before publishing the file.

## Uncited / heuristic claims to verify

- The ~200 line target and ~400 line ceiling are heuristics drawn from observed practice in the curated examples above. No spec sets these numbers.
- The "first-response SLA" recommendation is editorial. No canonical source sets a number; the chapter asks the project to be honest about whatever its actual median is.
- The "fresh container or VM" verification is borrowed from the README pre-publish rule and is editorial in this context.

## Cross-references

- README-level project introduction → [`readme-guide.md`](readme-guide.md).
- Repo meta files (CoC, SECURITY, FUNDING, GOVERNANCE, templates) → [`repo-meta-files.md`](repo-meta-files.md).
- Changelog conventions referenced from the commit-format section → [`changelog-guide.md`](changelog-guide.md).
- Voice and AI-fingerprint vocabulary → `front-end/brand-and-copywriting.md`.
