## CONTRIBUTING.md

`CONTRIBUTING.md` is the second contact a project has with a stranger. The README sells the project; `CONTRIBUTING.md` decides whether their first PR is clean or noisy, and whether they file a second one. A project without contribution rules collects drive-by patches that maintainers reject by hand, again and again, for the same reason. Write the rules once, link them from the PR template, and stop repeating yourself in review.

Hold it to the same standard as the README — terse, copy-paste-runnable, current.

## Audiences

In order of who the file must serve:

- **First-time external contributor.** Has never touched the repo. Does not know the test runner, the branch model, the commit format, or whether maintainers want issues filed before PRs. Will close the tab if the setup section fails.
- **Returning occasional contributor.** Knows the project but forgets the specifics. Wants a quick reference for "how do I run the linter again."
- **Maintainer reviewing a PR.** Uses the file as the canonical answer to "this PR doesn't follow our rules." If a rule is enforced in review but not written down, the contributor was set up to fail.

## File location

Per GitHub: `CONTRIBUTING.md` is recognized at `./CONTRIBUTING.md`, `.github/CONTRIBUTING.md`, or `docs/CONTRIBUTING.md`. GitHub surfaces a "Contributing guidelines" link on the issue and PR creation pages when any of these exists. Pick one location per repo. `.github/` is conventional for repos that already use that directory for templates and Actions; root is conventional otherwise.

## Required sections (in order)

Drop sections that genuinely do not apply (a solo project may not need a recognition section). Do not reorder.

1. **Title and welcome.** One sentence stating that contributions are accepted and what kinds. "Bug reports, fixes, docs, and translations are accepted; large feature work, open an issue first." Set the scope before the reader writes a 4,000-line PR you will reject.
2. **Code of Conduct pointer.** One line linking to `CODE_OF_CONDUCT.md`. Do not inline the policy.
3. **Where to ask, where to file.** Issue tracker for bugs and feature requests. Discussions or chat (Discord, Matrix, Zulip) for questions. Security goes through `SECURITY.md` — name it explicitly so reporters do not file a public issue for a vulnerability.
4. **Ways to contribute.** A short list: bug reports, fixes, documentation, triage, translations, examples. Tells the reader they do not have to write code to help.
5. **Development setup.** Copy-paste-runnable. Clone, install, run, test. Pin the runtime version. Name the package manager. If setup needs a system library (Postgres, Redis, ImageMagick), say so before the install command. Setup that fails on a fresh checkout is the single most common reason first-time contributors give up.
6. **Branch and fork model.** Fork-and-PR for outside contributors; branch-and-PR for members. Name the default branch (`main`). State whether long-lived feature branches, release branches, or trunk-based development apply.
7. **Testing.** The exact command. The expected runtime. What "green" looks like. If the suite has tiers (unit, integration, e2e), say which a contributor must run before opening a PR and which CI handles.
8. **Submitting a change.** Branch naming if enforced. Commit message format (Conventional Commits, plain prose, sign-off requirements). PR title format. PR description requirements (issue link, test evidence, screenshots for UI). Link to the PR template if one exists.
9. **Code review and merge.** Expected first-response SLA ("a maintainer will respond within X days"). What blocks merge (passing CI, one approval, conventional title, signed CLA). Squash, merge-commit, or rebase merge — name the policy.
10. **Style and linting.** One paragraph plus a link. Name the linter, the formatter, and the command that auto-fixes both. If the formatter and a written rule conflict, follow `AGENTS.md` rule 4 — call it out, do not paper over.
11. **Sign-off.** DCO (`git commit -s`), CLA, or neither. If a CLA is required, link the document and name what it covers in one sentence. Reporters who do not understand the legal ask will not sign.
12. **License inheritance.** "By contributing, you agree that your contributions will be licensed under the project's `LICENSE`." One sentence. Required for outside contributions to be redistributable.
13. **Recognition (optional).** How contributors are credited. `AUTHORS`, `CONTRIBUTORS`, all-contributors bot, GitHub's auto-generated list. Skip if the project does not maintain one.
14. **Maintainers and contact.** Who to ping for what. GitHub handles or team aliases, not personal email.

## Voice and tone

- Imperative for procedure: "Run `npm test`." Not "you should run."
- Second person for instructions; first-person plural ("we") only when describing maintainer behavior (review SLA, governance).
- No "simply," "just," "easy." A reader who finds the setup hard after being told it is easy stops trusting the file.
- Code voice (`backticks`) for every command, file, branch name, label, and CI job.
- Pin the example to a real version where it matters (`Node 20.x`, `Ruby 3.3`, `Postgres 16`).

## Commit conventions

State the convention explicitly. Three common choices:

- **Conventional Commits.** Header `<type>(<scope>): <description>`. Allowed types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`, `build`, `ci`, `style`. Breaking change marked `feat!:` or footer `BREAKING CHANGE: <description>`. Maps cleanly to semver — `fix` to PATCH, `feat` to MINOR, breaking to MAJOR. Pick this when you generate the changelog from commits.
- **Plain prose.** "Fix infinite loop in cache eviction." No prefix. Easier for new contributors; loses changelog automation.
- **Repo-local.** Whatever the project already uses. If `git log` already shows a pattern, document the existing pattern rather than imposing a new one mid-stream.

Show one example commit. A reader copies the example more reliably than they parse the rules.

## Issue and PR templates

Templates live in `.github/ISSUE_TEMPLATE/` (required path for GitHub to pick them up) and `.github/PULL_REQUEST_TEMPLATE.md`. Cross-reference templates from `CONTRIBUTING.md`; do not duplicate their contents. See `repo-meta-files.md` for template structure.

## Length and scope

- Target ~200 lines for a typical library; up to ~400 for a project with a non-trivial dev environment. Past that, split — `CONTRIBUTING.md` carries the rules, `docs/development.md` carries the deep dev guide.
- The file is for contribution mechanics, not project philosophy. Architectural rationale belongs in `ARCHITECTURE.md` or `docs/explanation/`.
- A long FAQ usually means the setup or PR sections are unclear. Fix the source.

## Anti-patterns

- **"PRs welcome" with no instructions.** Tells the contributor nothing and signals a maintainer who will reject their PR for unstated reasons.
- **Setup that does not run on a clean checkout.** The only valid test is pasting the commands into a fresh container or VM. Stale setup blocks every new contributor and is invisible to maintainers whose machines are already configured.
- **Hidden review rules.** "Please squash your commits." "We use Conventional Commits." If the rule is enforced, write it down. Surprising a contributor in review wastes their time and yours.
- **Listing every Conventional Commit type when only `feat` and `fix` are used.** Document what the project actually uses. Spec-sized rules for a hobby repo signal cargo cult.
- **Inlining the Code of Conduct.** Link it. The CoC has its own rules about modification (see `repo-meta-files.md`).
- **CLA without context.** A reader who does not know what a CLA is will not sign one. Name what it covers (copyright assignment, patent grant, redistribution rights) in one sentence and link the full document.
- **Stale "good first issue" labels.** Issues labeled `good-first-issue` that are actually three days of refactoring or that have already been claimed waste the goodwill of every newcomer who clicks them.
- **AI fingerprints.** "We are excited to welcome you to our vibrant community of passionate contributors." Strip every word that would not survive a code review.

## Pre-publish checklist

- [ ] Setup commands run end-to-end on a clean container.
- [ ] Test command runs and reports green on a clean checkout.
- [ ] Commit and PR conventions named, with one example.
- [ ] Sign-off mechanism named (DCO, CLA, none) and linked if applicable.
- [ ] License inheritance stated.
- [ ] Code of Conduct linked, not inlined.
- [ ] Security reporting points to `SECURITY.md`, not the public issue tracker.
- [ ] First-response SLA is honest. Do not promise 48 hours if the actual median is two weeks.
- [ ] `good first issue` label exists and at least one open issue carries it.
- [ ] Communication channels (chat, discussions) are monitored. A linked Discord with no maintainer presence is worse than no link.
- [ ] No `simply`, `just`, `easy`, `seamless`, `delve`, `leverage` in the prose.
- [ ] Length is under ~400 lines or overflow is moved to `docs/development.md`.

## Self-review

Before submitting, name three rules you enforce in review that are written down here, and one rule you will stop enforcing because it is not. Maintainer norms not in the file are bugs in the file.
