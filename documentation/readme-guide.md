## READMEs

The README is the first contact between a project and everyone else — users, contributors, future maintainers, your future self after six months away from the code. It decides whether a stranger spends the next ten minutes installing the project or closes the tab. Hold it to the same standard as the code: a sloppy README signals a sloppy project, regardless of what the source actually contains.

A reader scanning a README needs to answer four questions in under a minute: _what is this, does it solve my problem, can I use it, where do I learn more._ If any of those is unanswered above the fold, the README has failed.

## Audiences

Most READMEs serve three audiences in this order. Optimize for the first; do not exclude the others.

- **End user / integrator.** Wants to know what the project does and how to install and call it. Lands on the page from a search result or package registry. Skims.
- **Evaluator.** A senior engineer deciding whether to adopt this project versus alternatives. Looks for maturity signals: license, last release, test coverage, supported versions, who maintains it.
- **Contributor.** Wants to know how to run the project locally, where to file issues, and what conventions apply. Reads top-to-bottom only after deciding to invest.

If the project is internal-only, swap "end user" for "the next teammate onboarded to this repo" — the structure does not change.

## Required sections (in order)

This is the canonical order. It moves from broadest to most specific — readers who only need the first answer never have to scroll. Drop sections that genuinely do not apply (a CLI has no API; a private repo has no public license discussion). Do not reorder.

1. **Title.** Project name as an `<h1>`. Match the package name on npm/PyPI/crates.io exactly.
2. **One-line description.** Plain prose under 120 characters. Says what the project is and who it is for. No tagline, no marketing copy, no "blazing-fast." Match the description used on the package registry and on GitHub.
3. **Badges (optional).** Build status, latest version, coverage, license, package downloads. One line of badges. More than five reads as a trophy case. Place under the description, never inside it.
4. **Visual (optional).** A screenshot, terminal recording, or short GIF when it materially clarifies what the project does. CLIs benefit from a recorded session; libraries usually do not. Never put critical information only in an image — screen readers and text-mode terminals will miss it.
5. **Description / background.** Two or three short paragraphs. What problem the project solves, the design decision that distinguishes it from alternatives, and any context the reader needs before installing. Skip if the one-liner already says enough.
6. **Table of contents.** Required when the README exceeds roughly one screen (~100 lines). Otherwise the headings carry the navigation themselves.
7. **Install.** Copy-paste-runnable commands for the supported install methods, in order of how most users will install (package manager first, source build last). Pin the example to a real version of the package manager when the install is sensitive to it. Call out prerequisites (runtime version, system libraries) before the install command, not after.
8. **Usage / quickstart.** The minimum code that demonstrates the project doing its core job. Show the input, the call, and the expected output. Syntax-highlighted. If a single example cannot show the value, two are allowed; ten are a tutorial and belong in `docs/`.
9. **Configuration (when applicable).** Environment variables, config-file keys, CLI flags. A table beats prose. Document the default for every option.
10. **Project status / roadmap (optional).** "Alpha — API may change," "Maintained, accepting PRs," "Archived — see `<successor>`." A reader's tolerance for rough edges depends on knowing this. Lying about it costs trust permanently.
11. **Contributing.** One short paragraph plus a link to `CONTRIBUTING.md`. Long policy belongs in the dedicated file, not inline.
12. **Maintainers / authors / acknowledgments (optional).** Who owns the project, where to reach them, what prior work it builds on.
13. **License.** Last section, every time. Name the license (e.g., "MIT"), link to the `LICENSE` file. If the license is non-obvious (dual-licensed, AGPL, commercial), state the implication in one sentence.

## Voice and tone

- Write declarative sentences in the present tense. "Foobar pluralizes English nouns." Not "Foobar is a library that will help you to pluralize."
- Strip hedging. No "simply," "just," "easy," "blazing-fast," "powerful," "robust." A reader who finds the install hard after being told it is easy loses trust in the rest.
- Address the reader as "you" when giving instructions. Never "we" — the project is not a team meeting.
- Technical writing is still writing. Boring is not a virtue. Be terse, be specific, occasionally be funny — but not at the cost of clarity.
- One claim, one sentence. Long compound sentences hide weak claims.
- Show, don't describe. A three-line code block beats a paragraph explaining what the code would look like.
- Use code voice (`backticks`) for every file name, command, env var, function, and flag. Naked identifiers in prose are unreadable.

## Length and scope

- "As short as it can be without being any shorter." Anything that does not help a first-time reader install or evaluate the project does not belong in the README.
- Hard ceiling: if the file passes ~400 lines, you are writing documentation, not a README. Move material to `docs/`, link to it, and trim the README to what survives that move.
- The README is not the API reference, the design doc, the changelog, or the contributor handbook. Each of those gets its own file. Link to them.
- A `CHANGELOG.md` belongs next to the README, not inside it.
- A long FAQ usually means the install or usage section is unclear — fix the source of the questions before adding answers.

## Visuals and badges

- Badges signal maturity at a glance: license, latest version, build status, package downloads. Pick three to five that a reader actually uses for evaluation. Skip vanity badges (PRs welcome, made with love).
- Host badge images via `shields.io` or equivalent — they cache and stay live. Do not hot-link from CI vendor URLs that may change.
- Screenshots: include `alt` text. PNG for UI, SVG for diagrams, GIF or `vhs`-recorded terminal session for CLI demos. Keep file size under ~500 KB; large images push the install instructions below the fold.
- Animated GIFs autoplay and loop on GitHub. Use them sparingly and never for content that is not also written in prose.

## Anti-patterns

These appear in low-quality READMEs across every language and stack. Audit for them.

- **No description.** The repo title alone never answers "what is this." A surprising number of public projects ship like this.
- **Marketing voice.** "Revolutionary," "next-generation," "delightful developer experience." Strip every word that would not survive a code review.
- **AI fingerprints.** "Delve," "leverage," "seamless," "robust," "tapestry," em-dashes used as commas, three-bullet lists with parallel adjective-noun-verb structure, every section opening with "In today's fast-paced world." A README that reads as machine-generated signals that nothing in the repo has been carefully edited.
- **Wall of badges.** Twenty shields stacked four rows deep. Picks the wrong signal: process over substance.
- **Install instructions that don't run.** `npm install` with no package name. `pip install foo` when the package is named `foo-bar`. Always paste your own README's commands into a clean shell before publishing.
- **Usage example that omits the import.** A snippet starting at `foo.run()` with no `from foo import …` above it. The reader cannot tell where `foo` came from.
- **API documentation in the README.** Every method, every parameter. Generate API docs into `docs/` and link to them.
- **Stale roadmap.** "Coming Q3 2023" still in the README in 2026. Either delete the section or update it.
- **License section that says "see LICENSE."** Name the license. A reader scanning twenty alternatives needs to know it is MIT vs AGPL without opening another file.
- **Contributing section that is the entire `CONTRIBUTING.md` inlined.** Link out.
- **Emoji-heavy headers.** They tokenize poorly for screen readers and look dated within a year.

## Pre-publish checklist

Run before opening the PR. Each item is a binary pass/fail.

- [ ] Title matches the package-registry name.
- [ ] One-line description fits in 120 characters and matches the registry description.
- [ ] First screen answers: what, who-for, install, minimal usage.
- [ ] Every command was pasted into a clean shell and ran successfully.
- [ ] Every code block declares its language.
- [ ] No `simply`, `just`, `easy`, `blazing-fast`, `seamless`, `robust`, `delve`, `leverage` in the prose.
- [ ] License is named, not just linked.
- [ ] Badges are five or fewer and each one carries a real signal.
- [ ] Images have `alt` text and are under 500 KB.
- [ ] Roadmap and version-support claims are current as of today.
- [ ] Links resolve. No `TODO` or placeholder URLs.
- [ ] Total length under ~400 lines, or the overflow is moved to `docs/` and linked.

## Self-review

Before submitting any README, name three things you cut and one thing the reader will want next that you deliberately did not write. If you cannot name them, the draft is not finished.
