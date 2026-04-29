# Sources — repo-meta-files.md

Citations backing claims in [`repo-meta-files.md`](repo-meta-files.md). Developer artifact — not loaded by skill consumers. Update in the same commit when chapter rules change.

Per `CONTRIBUTING.md`:

- Human-authored sources only (specs, established engineering blogs, production codebases, books). No AI-generated content.
- Cite the specific claim, not just the topic.
- Mark uncited rules inline ("uncited — verify before publishing") rather than fabricating sources.

## Canonical references

### File locations and GitHub recognition

- [GitHub: Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file) — exhaustive list of GitHub-recognized community-health files and their accepted locations (`./`, `.github/`, `docs/`); also the rule that issue templates must live in `.github/ISSUE_TEMPLATE/`.
- [GitHub: About community profiles](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/about-community-profiles-for-public-repositories) — the community profile checker that surfaces missing files; backs the "fails silently when missing" framing.

### License

- [choosealicense.com (GitHub)](https://choosealicense.com/) — the canonical "pick a license" tool. Source of the "without a license, all rights reserved" claim.
- [SPDX License List](https://spdx.org/licenses/) — verbatim license texts and identifiers. Backs the "do not reword" rule.
- [GitHub: Licensing a repository](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository) — confirms `LICENSE` and `LICENSE.md` are both detected.

### Code of Conduct

- [Contributor Covenant 3.0](https://www.contributor-covenant.org/version/3/0/code_of_conduct/) — the canonical text and adoption rules. Backs the "do not modify the Standards section without removing attribution" rule.
- [Contributor Covenant adoption guide](https://www.contributor-covenant.org/adoption/) — placement guidance and the requirement to fill the enforcement contact.
- [GitHub: Adding a code of conduct to your project](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-a-code-of-conduct-to-your-project) — accepted file paths and GitHub's CoC chooser.

### Security policy

- [GitHub: Adding a security policy to your repository](https://docs.github.com/en/code-security/getting-started/adding-a-security-policy-to-your-repository) — the recognized `SECURITY.md` locations and the rule that the file should contain "supported versions" and "how to report a vulnerability."
- [GitHub: About coordinated disclosure of security vulnerabilities](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/about-coordinated-disclosure-of-security-vulnerabilities) — the coordinated-disclosure framing, the "private dialog" requirement, and the rationale behind embargo periods.
- [GitHub: Privately reporting a security vulnerability](https://docs.github.com/code-security/security-advisories/guidance-on-reporting-and-writing/privately-reporting-a-security-vulnerability) — the private vulnerability reporting feature referenced as the recommended channel.
- [The GitHub Blog — Coordinated vulnerability disclosure (CVD) for open source projects](https://github.blog/security/vulnerability-research/coordinated-vulnerability-disclosure-cvd-open-source-projects/) — backs the 90-day embargo as a common default and the rule that maintainers must publish a real reporting channel rather than rely on `git log` extraction.
- [CERT/CC Vulnerability Disclosure Guide](https://vuls.cert.org/confluence/display/CVD/Executive+Summary) — independent backing for coordinated-disclosure timelines.

### Funding

- [GitHub: Displaying a sponsor button in your repository](https://docs.github.com/en/sponsors/integrating-with-github-sponsors/displaying-a-sponsor-button-in-your-repository) — the canonical list of supported `FUNDING.yml` platforms and the strict `.github/FUNDING.yml` path requirement.

### Issue and PR templates

- [GitHub: About issue and pull request templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/about-issue-and-pull-request-templates) — required paths (`.github/ISSUE_TEMPLATE/`, `.github/PULL_REQUEST_TEMPLATE.md`).
- [GitHub: Configuring issue templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository) — the `config.yml` mechanism, `blank_issues_enabled: false`, and contact-link routing.
- [GitHub: Syntax for issue forms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms) — the YAML schema for issue forms; backs the "forms over markdown templates" recommendation.

### CODEOWNERS

- [GitHub: About code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners) — accepted paths, the "last matching rule wins" semantics, and the integration with branch protection.

### Citation

- [Citation File Format spec (citation-file-format)](https://citation-file-format.github.io/) — schema and required fields for `CITATION.cff`.
- [GitHub: About citation files](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-citation-files) — surfaces the "Cite this repository" button when present.

### Governance

- [Open Source Guides — Leadership and governance](https://opensource.guide/leadership-and-governance/) — backs the role definitions, decision-making models, and "document actual practice, not aspiration" rule.
- [CNCF Project Governance Best Practices](https://contribute.cncf.io/maintainers/governance/) — corroborates the role list and conflict-resolution requirement for foundation-track projects.

### AUTHORS / CONTRIBUTORS

- [all-contributors specification](https://allcontributors.org/docs/en/specification) — backs the "maintain by bot or by hand; do not maintain partially" recommendation.

## Tool catalog

- [GitHub Community Standards checker](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/about-community-profiles-for-public-repositories) — the per-repo `Insights → Community Standards` page that flags missing files.
- [Probot template suite](https://probot.github.io/) — automation for several of the meta-file workflows (issue routing, stale management, CoC enforcement bots).
- [Allcontributors bot](https://github.com/all-contributors/all-contributors-bot) — maintains the `CONTRIBUTORS` list automatically.
- [Zenodo](https://zenodo.org/) — DOI minting for `CITATION.cff` integration.
- [SECURITY.md template (electron/.github)](https://github.com/electron/.github/blob/main/SECURITY.md) — production-grade template; cited as a starting point.

## Curated examples

- [rust-lang/.github](https://github.com/rust-lang/.github) — org-level defaults; demonstrates the public `.github` repo pattern.
- [microsoft/.github SECURITY.md](https://github.com/microsoft/.github/blob/main/SECURITY.md) — large-org security policy with bug-bounty and platform-routing.
- [angular/angular CODEOWNERS](https://github.com/angular/angular/blob/main/.github/CODEOWNERS) — large-scale CODEOWNERS file using teams over individuals.
- [home-assistant/core CODEOWNERS](https://github.com/home-assistant/core/blob/dev/CODEOWNERS) — the `CODEOWNERS` model for a project with hundreds of integration submodules.
- [vercel/next.js issue templates](https://github.com/vercel/next.js/tree/canary/.github/ISSUE_TEMPLATE) — exemplary issue forms with reproduction-link enforcement.

## Uncited / heuristic claims to verify

- The "audit `CODEOWNERS` quarterly" cadence is editorial. No source sets a number; the rule generalizes the "stale references silently block PRs" failure mode.
- The "60-line PR template ceiling" and "50-line SUPPORT.md ceiling" are heuristics drawn from observed templates in the curated examples above.
- The "GOVERNANCE.md only when more than two maintainers" threshold is editorial.
- The "`AUTHORS` for legal copyright; `CONTRIBUTORS` for broader recognition" distinction is convention, not specification — it varies by project, especially for FSF-adjacent and Apache projects which have stricter `AUTHORS` semantics.

## Cross-references

- README structure → [`readme-guide.md`](readme-guide.md).
- CONTRIBUTING and PR-template integration → [`contributing-guide.md`](contributing-guide.md).
- `Security` section in changelog → [`changelog-guide.md`](changelog-guide.md).
- Voice and AI-fingerprint vocabulary → `front-end/brand-and-copywriting.md`.
- Security audit conventions for the policy itself → `security/AGENTS.md` (when populated).
