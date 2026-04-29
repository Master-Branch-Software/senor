# Sources — docs-site-structure.md

Citations backing claims in [`docs-site-structure.md`](docs-site-structure.md). Developer artifact — not loaded by skill consumers. Update in the same commit when chapter rules change.

Per `CONTRIBUTING.md`:

- Human-authored sources only (specs, established engineering blogs, production codebases, books). No AI-generated content.
- Cite the specific claim, not just the topic.
- Mark uncited rules inline ("uncited — verify before publishing") rather than fabricating sources.

## Canonical references

- [Diátaxis — Daniele Procida](https://diataxis.fr/) — the canonical statement of the four-mode framework. Source of the tutorial / how-to / reference / explanation distinction, the action-vs-cognition × acquisition-vs-application matrix, and the rule that documentation should be organized around the four user needs.
- [Diátaxis — Start here (five-minute summary)](https://diataxis.fr/start-here/) — concise summary; backs the per-mode purpose statements ("a tutorial is a lesson," "a how-to addresses a real-world goal," etc.).
- [Diátaxis — How to use Diátaxis](https://diataxis.fr/how-to-use-diataxis/) — backs the "do not restructure all at once" guidance. The chapter's incremental-migration framing follows directly. Procida explicitly warns against creating empty mode folders ("don't do that, it's horrible").
- [Diátaxis — Tutorials](https://diataxis.fr/tutorials/), [How-to guides](https://diataxis.fr/howto-guides/), [Reference](https://diataxis.fr/reference/), [Explanation](https://diataxis.fr/explanation/) — per-mode rules including "tutorials must be reproducible," "how-to titles state the goal," "reference must be neutral," "explanation argues a position."
- [Write the Docs — What is the Diátaxis framework?](https://www.writethedocs.org/videos/portland/2017/the-four-kinds-of-documentation-and-why-you-need-to-understand-what-they-are-daniele-procida/) — Procida's original talk. Background and rationale.
- [Canonical: Diátaxis, a new foundation for Canonical documentation](https://ubuntu.com/blog/diataxis-a-new-foundation-for-canonical-documentation) — large-scale adoption case study; backs the claim that mode separation works at the scale of an OS-level documentation set.

## Site framework references

- [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) — feature reference, search behavior, versioning plugin (`mike`).
- [Docusaurus](https://docusaurus.io/docs) — multi-version documentation feature; i18n; plugin model.
- [Sphinx documentation](https://www.sphinx-doc.org/) — autodoc behavior, reStructuredText surface area; backs "Sphinx for Python autodoc."
- [mdBook](https://rust-lang.github.io/mdBook/) — Rust-friendly book toolchain; backs the linear-book strength.
- [Astro Starlight](https://starlight.astro.build/) — modern static-site framework feature set.
- [VitePress](https://vitepress.dev/) — Vue-aligned static site generator.
- [Read the Docs documentation](https://docs.readthedocs.io/) — versioning model, hosted PDF/ePub builds, multi-format publication.

## Reference-generation tooling

- [Sphinx autodoc](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html) — Python source-to-reference generator.
- [TypeDoc](https://typedoc.org/) — TypeScript reference generator from JSDoc/TSDoc comments.
- [rustdoc](https://doc.rust-lang.org/rustdoc/) — Rust reference generator; treats `///` doc comments as Markdown.
- [godoc / pkg.go.dev](https://pkg.go.dev/) — Go's identifier-comment convention surfaced as a hosted reference.
- [Redoc](https://github.com/Redocly/redoc) and [Stoplight Elements](https://stoplight.io/open-source/elements) — OpenAPI-to-reference rendering.

## Search

- [Algolia DocSearch](https://docsearch.algolia.com/) — free for OSS docs; cited as the recommended hosted search.
- [Pagefind](https://pagefind.app/) — static-site full-text search alternative; runs without a hosted backend.
- [MkDocs Material search plugin](https://squidfunk.github.io/mkdocs-material/setup/setting-up-site-search/) — built-in option.

## Linting and link-audit tooling

- [lychee](https://github.com/lycheeverse/lychee) — broken-link checker; used in CI.
- [Vale](https://vale.sh/) — prose linter with style-guide rule packs.
- [write-good](https://github.com/btford/write-good) — naive prose linter; catches passive voice, weasel words.
- [markdownlint](https://github.com/DavidAnson/markdownlint) — Markdown style checker.
- [mkdocs-linkcheck](https://github.com/byrnereese/mkdocs-linkcheck) — MkDocs-native link auditing.

## Diagrams

- [Mermaid](https://mermaid.js.org/) — text-to-diagram syntax; renders inline in most doc frameworks and on GitHub.
- [PlantUML](https://plantuml.com/) — alternative text-to-diagram syntax with broader UML support.

## Curated examples

Documentation sites worth studying. Each gets at least one specific aspect right.

- [Stripe API reference](https://stripe.com/docs/api) — gold-standard reference structure: signature, parameters, returns, example, side-by-side response panel.
- [Tailwind CSS docs](https://tailwindcss.com/docs) — strong reference + how-to integration; clean version-switch UI.
- [Django documentation](https://docs.djangoproject.com/) — explicitly four-mode structure; cited in Diátaxis as a reference adoption.
- [Kubernetes docs](https://kubernetes.io/docs/) — large-scale four-mode site; demonstrates the model at OS-level scope.
- [Rust Book](https://doc.rust-lang.org/book/) — paradigmatic mdBook tutorial.
- [Astro docs](https://docs.astro.build/) — well-maintained Starlight site; demonstrates the modern static-site stack end-to-end.
- [FastAPI docs](https://fastapi.tiangolo.com/) — tutorial-heavy, illustrates the "tutorial as the primary marketing tool" pattern.

## Uncited / heuristic claims to verify

- The "~400-line README threshold" for spinning up a docs site mirrors the README chapter's own limit and is editorial when applied at the site-creation decision.
- The "~10 functions / endpoints" and "~10 configuration keys" thresholds are heuristics drawn from observed practice.
- The "two-level sidebar tree" rule is editorial; it generalizes UX conventions for navigation but no canonical doc-site source sets the limit.
- The "test code samples in CI" recommendation is widely shared but does not have a single canonical source. Sphinx doctest, mdBook test runner, and `mdsh` all implement it.
- The "AI fingerprint vocabulary" is cross-referenced from `front-end/brand-and-copywriting.md`; this chapter does not duplicate the full list.

## Cross-references

- Voice and writing rules per mode → `copywriting/technical-writing.md`.
- README pointing at the docs site → [`readme-guide.md`](readme-guide.md).
- Inline code documentation that feeds generated reference → [`inline-code-docs.md`](inline-code-docs.md).
- Internationalization for translated docs sites → `front-end/internationalization.md`.
- Voice and AI-fingerprint vocabulary → `front-end/brand-and-copywriting.md`.
