## Repo meta files

The files outside `README` and `CONTRIBUTING` that ship with a project and shape how strangers interact with it. Code of Conduct, Security policy, Support, Funding, Governance, issue and PR templates, license. Each is short. Each has a canonical location. Each fails silently when missing — the contributor or reporter does the wrong thing because the right thing was undocumented, and the maintainer pays the cost.

This chapter covers what each file is, where it lives, and what it must contain. For the README and CONTRIBUTING files, see [`readme-guide.md`](readme-guide.md) and [`contributing-guide.md`](contributing-guide.md).

## Where these files live

GitHub recognizes community-health files in three locations: `./`, `.github/`, or `docs/`. Pick one location per repo and stay consistent. The exception is issue and PR templates, which must live in `.github/ISSUE_TEMPLATE/` and `.github/PULL_REQUEST_TEMPLATE.md` to be picked up by the GitHub UI.

A user account or organization can also publish defaults in a public `.github` repository. Any community-health file missing from a project repo is filled from there. Useful for large orgs; ignore for individual projects.

| File                       | Location                                      | Required for                             |
| -------------------------- | --------------------------------------------- | ---------------------------------------- |
| `LICENSE`                  | root                                          | Every public repo                        |
| `README.md`                | root                                          | Every repo                               |
| `CONTRIBUTING.md`          | root, `.github/`, or `docs/`                  | Any repo accepting outside contributions |
| `CODE_OF_CONDUCT.md`       | root, `.github/`, or `docs/`                  | Any community-facing repo                |
| `SECURITY.md`              | root, `.github/`, or `docs/`                  | Any repo where vulnerabilities matter    |
| `SUPPORT.md`               | root, `.github/`, or `docs/`                  | Repos with external users                |
| `GOVERNANCE.md`            | root, `.github/`, or `docs/`                  | Multi-maintainer projects, foundations   |
| `FUNDING.yml`              | `.github/`                                    | Repos with sponsorship                   |
| `CODEOWNERS`               | `.github/`, root, or `docs/`                  | Any repo using review automation         |
| Issue templates            | `.github/ISSUE_TEMPLATE/` (required)          | Repos with structured issues             |
| PR template                | `.github/PULL_REQUEST_TEMPLATE.md` (required) | Repos with structured PRs                |
| `AUTHORS` / `CONTRIBUTORS` | root                                          | Recognition-tracking projects            |
| `CITATION.cff`             | root                                          | Academic / scientific software           |

## LICENSE

Without a license, a public repo defaults to "all rights reserved." Nobody can use, fork, or contribute legally. License is not optional for any project that wants users.

- File name: `LICENSE` (no extension) or `LICENSE.md`. GitHub detects either.
- Copy the verbatim license text from the upstream — `choosealicense.com` for picking, `spdx.org/licenses/` for the canonical text. Do not reword.
- Update the year and copyright holder line, nothing else.
- One license per repo. Multi-licensed projects (e.g., MIT + Apache, common in Rust) ship `LICENSE-MIT` and `LICENSE-APACHE` and explain the choice in the README.
- Do not invent licenses. "MIT but you cannot use it commercially" is not MIT and is not a license.

## CODE_OF_CONDUCT.md

Sets behavioral expectations and the enforcement path. The default text most projects use is the Contributor Covenant.

- Adopt **Contributor Covenant 3.0** unless a reason exists not to. Link: `contributor-covenant.org`. Most foundations (CNCF, Apache, PSF) require it or an equivalent.
- Three sections are non-optional: **Our Standards** (what is acceptable, what is not), **Enforcement** (who handles reports, how to reach them), and **Scope** (where the CoC applies — repo, issues, chat, conferences).
- Replace the placeholder enforcement contact (`[INSERT CONTACT METHOD]`) with a real, monitored channel. A CoC with a bracketed placeholder is signal that nobody owns enforcement and the file is a sticker.
- Do not modify the Standards section. Modified covenants are no longer Contributor Covenant; remove the attribution if you change the body.
- Link the CoC from `CONTRIBUTING.md` and the project welcome page. Do not inline.

## SECURITY.md

The file a vulnerability reporter reads before deciding whether to disclose to the project or to a CVE database. If reporting is unclear, reporters extract maintainer emails from `git log` or, worse, file public issues.

Required sections:

1. **Supported versions.** A small table. Which versions still get patches. Be honest — listing every version as "supported" when only `main` is patched costs trust the first time someone reports against an older release.
2. **Reporting channel.** Either GitHub's private vulnerability reporting (enabled in Settings → Security → "Private vulnerability reporting"), an email address (a project alias, not a personal account), or a third-party platform (HackerOne, huntr, Bugcrowd). Pick one. Do not direct reporters to the public issue tracker.
3. **Expected response time.** A real number. "Acknowledgment within 72 hours, fix or status update within 14 days" is verifiable. "We take security seriously" is not.
4. **Disclosure policy.** Coordinated disclosure is the default. Name the embargo period (commonly 90 days). Reference any CVE-issuing process if the project has one.
5. **Scope (optional).** What counts as a vulnerability. Useful for projects that ship with deliberate trust assumptions (developer tools, sandboxes).

Anti-patterns: directing reports to a personal email, no response-time commitment, listing one supported version while still publishing security advisories for older ones, files copied from a template with `[YOUR EMAIL]` left in place.

For the changelog's `Security` entry rules, see [`changelog-guide.md`](changelog-guide.md) § Required structure.

## SUPPORT.md

Where users go for help that is not a bug report. GitHub surfaces this on the issue-creation page. Without it, users file usage questions as bugs and maintainers triage them by hand.

- Name the actual channel. Discord, Discussions, Stack Overflow tag, mailing list, paid support tier.
- State what is and is not supported. "Bug reports go to the issue tracker. Usage questions go to GitHub Discussions. We do not support installation on macOS 10.14 or earlier."
- Link the documentation. Most "support" requests are documentation gaps.
- Keep it under 50 lines. Long support docs belong in `docs/troubleshooting.md`.

## GOVERNANCE.md

Required when the project has more than two maintainers or accepts contributions from people who could become maintainers. Names how decisions are made, who can merge, and how new maintainers are added.

Sections worth including:

- **Roles.** Contributor, committer, maintainer, lead. Define each in one sentence. Name what privileges each role grants.
- **Decision-making.** Lazy consensus, majority vote, BDFL, core-team vote. State the model and the threshold.
- **Adding and removing maintainers.** The process. The criteria. The probationary period if there is one.
- **Conflict resolution.** What happens when maintainers disagree. Who breaks ties.
- **Foundation or sponsoring entity.** If applicable, name it and link the relevant agreements.

Skip if the project has one maintainer. A `GOVERNANCE.md` with one name on it signals fragility, not transparency.

## FUNDING.yml

GitHub-specific. Powers the "Sponsor" button on the repo page.

- Path: `.github/FUNDING.yml`. No other location works.
- Format: a YAML file mapping platform names to handles. Supported platforms: `github`, `patreon`, `open_collective`, `ko_fi`, `tidelift`, `community_bridge`, `liberapay`, `issuehunt`, `lfx_crowdfunding`, `polar`, `buy_me_a_coffee`, `thanks_dev`, `custom`.
- One handle per platform. The `custom` field accepts up to four URLs.
- A funding link in the README is fine, but `FUNDING.yml` is what enables the Sponsor button — they are not redundant.

## Issue templates

Live in `.github/ISSUE_TEMPLATE/`. Without templates, every bug report is a different shape and triage is manual.

- Use **issue forms** (YAML) over **markdown templates**. Forms render as structured fields with validation; markdown templates render as a freeform editor with a prefilled body that users can wipe.
- One template per intent: `bug_report.yml`, `feature_request.yml`, `documentation.yml`. Do not stack three intents into one form.
- Required fields: a reproduction (for bugs), an expected vs actual outcome (for bugs), the version and environment, and a checkbox confirming the reporter searched existing issues. Optional everything else.
- Add a `config.yml` to disable blank issues (`blank_issues_enabled: false`) and route off-topic reports to the right channel (Discussions, security policy, support docs).

## PR template

Lives at `.github/PULL_REQUEST_TEMPLATE.md`. One template per repo by default; multiple templates supported via the `?template=name.md` query parameter.

Sections that earn their keep:

- **Summary.** What changed and why. Two or three sentences.
- **Linked issue.** `Closes #123`. Required for non-trivial changes.
- **Type of change.** Checkbox list mirroring the project's commit conventions (feature, fix, docs, refactor, breaking).
- **Testing.** What the contributor ran, what passed. Not a courtesy — review skips faster when this is filled.
- **Checklist.** Three to five items maximum. Pre-publish checklist for the project — tests, changelog entry, docs updated, sign-off. A twenty-item checklist is ignored.

Keep the template under 60 lines. A PR template longer than the average PR diff is a process bug.

## CODEOWNERS

Routes review automatically. Lines are `<glob> <@user-or-team> <@user-or-team>`.

- Path: `.github/CODEOWNERS`, `CODEOWNERS` at root, or `docs/CODEOWNERS`. Pick one.
- One rule per line. Last matching rule wins — order matters.
- Use teams (`@org/team-name`) over individuals where possible. Individuals leave; teams persist.
- Pair with branch protection's "Require review from Code Owners" rule. Without that, `CODEOWNERS` is documentation, not enforcement.
- Audit quarterly. Stale code owners (a person who left two years ago, a team that was dissolved) silently block every PR that touches their paths.

## CITATION.cff

For academic and scientific software. Tells researchers how to cite the project in papers.

- Format: Citation File Format (`citation-file-format` spec). YAML.
- Path: `CITATION.cff` at root.
- GitHub renders a "Cite this repository" button when present.
- Name the authors, the version, the DOI (if minted via Zenodo), and the year.
- Skip for non-academic projects.

## AUTHORS / CONTRIBUTORS

Recognition. Two conventions:

- `AUTHORS` — original authors and substantial contributors. Often legally significant for copyright attribution.
- `CONTRIBUTORS` — broader list, anyone who landed a non-trivial change.

Maintain by hand or via the all-contributors bot. Skip both if the project does not actively maintain a list — a stale `CONTRIBUTORS` file omitting the last fifty contributors is worse than no file.

## Anti-patterns

- **Templates with placeholders.** `[INSERT EMAIL]`, `[YOUR ORGANIZATION]`, `<!-- TODO -->`. Each one signals the file was copied and never finished.
- **Conflicting locations.** `CODE_OF_CONDUCT.md` in both root and `.github/`. GitHub picks one (root wins). The other rots.
- **Files that contradict each other.** `SECURITY.md` says "email security@", `README.md` says "open an issue for vulnerabilities." Pick one path and align every file.
- **CoC with no enforcement contact.** Or an enforcement contact pointing to a personal account that nobody monitors.
- **Issue templates that ask for thirty fields.** Reporter abandons the form. Three required fields beats fifteen optional ones.
- **PR templates that forbid the contributor from editing them.** Contributors will edit them anyway. Write the template assuming sections will be deleted when not relevant; mark the few that must stay.
- **`FUNDING.yml` pointing to a closed Patreon or expired sponsor link.** Audit annually.
- **Inlining license text in the README.** Link the `LICENSE` file. Inlined license text in another file means two copies that drift.
- **Governance file written aspirationally.** Document the actual decision-making, not the model the project intends to adopt next quarter.

## Pre-publish checklist (per file)

Run when adding or auditing any meta file:

- [ ] File is in a GitHub-recognized location.
- [ ] No placeholder text (`[YOUR EMAIL]`, `<!-- TODO -->`, `lorem ipsum`).
- [ ] Every contact is a monitored channel, not a personal account.
- [ ] Every cross-reference resolves (links, issue templates routing to the right place, `SECURITY.md` referenced from `CONTRIBUTING.md`).
- [ ] Versions and dates are current.
- [ ] License text is the verbatim upstream, not a reworded version.
- [ ] Issue and PR templates render correctly in the GitHub UI (paste the template path into the issue-creation URL to test).
- [ ] If `CODEOWNERS` exists, every team and user reference is current.

## Self-review

Name one meta file you added and one you deleted. The first is the rule the project now enforces; the second is the rule the project pretended to enforce but did not. Both are wins.
