## Documentation site structure

When a project outgrows its README, the next step is a `docs/` directory or a separate documentation site. The temptation is to dump everything into one folder organized by feature. The result is a site where the same person reads the same page three times — first to learn, then to do, then to look up — and gets a different answer each time, none of which fully fits the need.

This chapter covers how to lay out a documentation site so that each page does one job. For the voice rules of each page type, see `copywriting/technical-writing.md`. For the README that points at the site, see [`readme-guide.md`](readme-guide.md).

## Diátaxis: organize by mode, not by feature

Diátaxis (Daniele Procida) is the standard model. Four modes, distinguished by what the reader is doing and what they are after:

| Mode        | Reader is     | Need                   | Example                                    |
| ----------- | ------------- | ---------------------- | ------------------------------------------ |
| Tutorial    | Studying      | Learn by doing         | "Build your first widget"                  |
| How-to      | Working       | Solve a specific goal  | "Add authentication to an existing app"    |
| Reference   | Looking up    | Get a fact, accurately | API surface, configuration keys, CLI flags |
| Explanation | Understanding | Build a mental model   | "Why we chose Postgres over MySQL"         |

Two axes: the reader is acquiring or applying skill, and is studying or working. Each mode lives in one cell.

The site's top-level navigation is the four modes — not the project's feature taxonomy. Feature pages exist within each mode (a `databases` how-to and a `databases` reference page are different documents in different folders).

```
docs/
  tutorials/
    01-getting-started.md
    02-your-first-widget.md
  how-to/
    add-authentication.md
    deploy-to-kubernetes.md
    migrate-from-v2.md
  reference/
    cli/
    api/
    configuration.md
  explanation/
    why-postgres.md
    architecture.md
```

A landing page at `docs/index.md` directs the reader to the right mode based on what they are trying to do. "I am new" → tutorials. "I have a goal" → how-to. "I need a fact" → reference. "I want to understand why" → explanation.

## When to spin up a docs site

The README handles projects up to ~400 lines of prose. Past that, move material to `docs/`. Heuristics for the move:

- **Multiple install paths.** Docker, source build, package manager, Helm chart. Each gets its own how-to.
- **Non-trivial API surface.** More than ~10 functions or endpoints. Reference goes in `docs/reference/`, generated where possible.
- **Configuration with more than ~10 keys.** Configuration reference becomes its own page.
- **Tutorial demand.** First-time users keep asking the same setup question. Write the tutorial once, link it.
- **Architectural questions.** Reviewers, evaluators, and senior engineers keep asking "why X over Y." Explanation page.

A `docs/` folder rendered as Markdown on GitHub serves small projects fine. A dedicated site (MkDocs, Docusaurus, Sphinx, Astro Starlight, mdBook, VitePress, ReadTheDocs) earns its keep when the project needs search, versioning, or a navigation tree deeper than one level.

## Site framework selection

Pick once, change rarely. Migration costs grow with the size of the corpus.

| Stack                     | Strength                                                | Trade-off                                             |
| ------------------------- | ------------------------------------------------------- | ----------------------------------------------------- |
| Plain Markdown in `docs/` | Zero build, GitHub renders it                           | No search, no versioning, no cross-linking validation |
| **MkDocs Material**       | Best default for non-Python projects under 500 pages    | Python toolchain                                      |
| **Docusaurus**            | React-heavy projects, multi-version, i18n               | React lock-in, heavier than needed for small docs     |
| **Sphinx**                | Python projects, autodoc generation, scientific writing | reStructuredText is a learning curve                  |
| **mdBook**                | Rust projects, books with linear flow                   | Less suited to reference-heavy sites                  |
| **Astro Starlight**       | Modern static site, fast builds, multi-framework        | Newer, smaller plugin ecosystem                       |
| **VitePress**             | Vue projects, fast dev loop                             | Vue lock-in                                           |
| **Read the Docs**         | Hosted Sphinx with versioning and PDF                   | Opinionated, hosted-only                              |

Decide on three axes: the project's primary language, whether multi-version docs are needed, and whether contributors will write Markdown or accept a different markup. Mismatch any of the three and the site fights the team for years.

## Information architecture

### Top-level navigation

Four entries, mirroring the four Diátaxis modes. Add a fifth only when needed:

- Tutorials
- How-to guides
- Reference
- Explanation
- (Optional) Release notes / changelog
- (Optional) FAQ — only when explanation pages have failed

Avoid feature-based top-level nav (`Database`, `Auth`, `Caching`). The reader does not browse by feature; they browse by what they are trying to do.

### Tutorials

- Numbered. The order is the curriculum.
- Each tutorial promises one outcome and delivers it. "By the end of this tutorial, your widget renders." A tutorial that ends with "next steps" but no working artifact has failed.
- One execution path per tutorial. No `if macOS … if Linux … if Windows` branching — that is three tutorials.
- Strict prerequisites. State them before step one. Missing prerequisites are the single largest source of tutorial abandonment.

### How-to guides

- Title is the goal as a task. "Migrate from v2 to v3." Not "Migration."
- One paragraph at the top stating when this guide applies and when it does not. Saves the reader from following a guide that is not for them.
- Steps as imperative verbs.
- Verification at the end. The reader knows whether they succeeded.
- Cross-link to reference for the precise option list, not to other how-tos.

### Reference

- Generated where possible. Hand-written reference rots faster than the code. Generators (Sphinx autodoc, TypeDoc, rustdoc, godoc, OpenAPI tooling) extract from source and stay current.
- One file per logical unit (module, package, class). Reference pages are search-result destinations, not narrative.
- Same shape every time. For an API: signature, parameters, returns, raises, example, see-also. Consistency is the feature.
- No editorializing. Reference is facts. Argument and rationale belong in explanation.

### Explanation

- Title names the question. "Why we chose Postgres over MySQL." Not "Database choice."
- Argues a position. The reader expects a viewpoint.
- Compare alternatives honestly. Hiding the trade-offs makes the argument less persuasive, not more.
- Cross-link out at the end. Explanation rarely stands alone — it leads back to a how-to or reference.

## Versioning

Documentation rots faster than code. Pick a versioning policy before the docs accumulate:

- **No versioning.** Docs track the latest release. Acceptable for small projects with stable APIs and active maintenance. Ship a "Last updated" date and pin examples to a real version.
- **Branch-per-version.** Old major versions get their own URL (`/v2/`, `/v3/`). Required for any project still patching old majors.
- **Versioned pages within one site.** Docusaurus, MkDocs Material, and Read the Docs all support this. Heavier to maintain.

Mark deprecation explicitly. A banner at the top of the page: "Deprecated in v3. See `<new page>`." Keep deprecated pages around — readers arrive from search results and stale bookmarks.

Date every page. Footer line: "Last updated `YYYY-MM-DD`." Either generated from git or written by hand. Without dates, doc rot is invisible.

## Search, navigation, and discoverability

- **Search is non-negotiable** at scale. A docs site over ~50 pages without working search is a site nobody can find anything in. Algolia DocSearch is free for open-source projects; most static-site doc frameworks ship a built-in alternative.
- **Sidebar navigation** mirrors the four-mode structure at the top, with a flat list of pages within each mode. Trees deeper than two levels are harder to scan than a flat list — collapse them.
- **In-page table of contents** for any page over ~600 words. Most doc frameworks render this automatically from headings.
- **Cross-links** within prose, code-voice for symbols. A reference page that does not link to its tutorial loses the reader who arrived by search.
- **Anchor stability.** Heading anchors are URLs. Renaming a heading breaks every external link to it. Add a redirect or keep the old anchor.

## Code samples in docs

Code in docs is docs. Same standards as the README:

- Language declared on every block.
- Pinned versions where they matter.
- Realistic identifiers (`customer_id`, `email`), not `foo` and `x`.
- Show the output where the output is what the reader is checking.
- Test in CI. Doctests, snapshot tests, executable Markdown — pick a tool and wire it. Untested code samples drift; drifted samples cost more goodwill than no samples.

## Diagrams and images

- Mermaid or PlantUML in source. Renders inline; survives dark-mode toggles; diffs cleanly. Static SVG only for diagrams the framework cannot generate.
- Screenshots only for UI walkthroughs. They rot when the UI changes. Prefer text representations of terminal output.
- Alt text is descriptive, not labeling. "Architecture diagram" is not alt text. "Web tier sends to API tier; API tier reads/writes Postgres and publishes to Kafka" is.
- File size budget under ~500 KB per image to keep pages cacheable on slow networks.

## Cross-cutting infrastructure

A docs site at scale needs:

- **Broken-link audit** in CI. `lychee` or `mkdocs-linkcheck` catch rot the moment a link dies.
- **Anchor-link audit.** Heading rename detection. Most static-site frameworks ship a plugin.
- **Vale or write-good** for prose linting. Catches AI fingerprints and hedging language before review.
- **A "Suggest an edit" link** on every page, pointing at the source file in the repo. Lowers the barrier to contributor fixes.
- **A "Last updated" timestamp** generated from git, surfaced in the page footer.

## Anti-patterns

- **One folder per feature, no mode separation.** "Database," "Auth," "Caching." Each contains tutorial, how-to, reference, and explanation jammed together. The reader cannot find anything.
- **Tutorials that branch on platform.** Three OSes, two package managers, four runtime versions in one tutorial. The reader hits a dead end on the path that does not match their setup.
- **Reference written as prose.** Long paragraphs where a parameter table would do. Reference is scanned, not read.
- **How-to written as tutorial.** Patiently explaining the basics to a reader who already knows the project. Wrong audience, wasted time.
- **Mode mixing in one page.** A page that opens "let's learn together" and ends "see also: configuration reference" is two pages.
- **Stale screenshots.** UI shipped six months ago; screenshots show three UIs back. Replace with text or delete.
- **No search.** A 200-page site relying on the sidebar.
- **Hand-written API reference for a generator-friendly language.** Drift starts on day one.
- **Aspirational structure with empty folders.** Empty `tutorials/` and `explanation/` folders signal a site that started, planned, and never delivered.
- **AI fingerprints.** "Comprehensive guide," "delve," "leverage," "robust solution," every page opening with the same three-bullet "what you'll learn." Strip the boilerplate.

## Pre-publish checklist

- [ ] Top-level navigation has four to six entries, no more.
- [ ] Every page belongs to exactly one Diátaxis mode.
- [ ] Tutorials run end-to-end on a clean machine.
- [ ] Reference is generated from source where the language supports it; hand-written reference has a freshness date and an owner.
- [ ] Search works and returns relevant results for the top ten user queries.
- [ ] Every page has a "Last updated" timestamp.
- [ ] Every code sample declares its language, pins versions where relevant, and is tested in CI.
- [ ] Broken-link audit passes.
- [ ] No empty placeholder folders or `coming soon` pages.
- [ ] No `simply`, `just`, `easy`, `seamless`, `delve`, `leverage` in the prose.
- [ ] "Suggest an edit" link on every page.
- [ ] Versioning policy is documented and visible to the reader.

## Self-review

Before shipping a new docs site or a major restructure, name one page that should be deleted, one that belongs in a different mode, and one mode the site is missing. If you cannot name them, the structure is not yet honest about what the project documents.
