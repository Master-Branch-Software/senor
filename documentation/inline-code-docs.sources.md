# Sources — inline-code-docs.md

Citations backing claims in [`inline-code-docs.md`](inline-code-docs.md). Developer artifact — not loaded by skill consumers. Update in the same commit when chapter rules change.

Per `CONTRIBUTING.md`:

- Human-authored sources only (specs, established engineering blogs, production codebases, books). No AI-generated content.
- Cite the specific claim, not just the topic.
- Mark uncited rules inline ("uncited — verify before publishing") rather than fabricating sources.

## Canonical references

### Python

- [PEP 257 — Docstring Conventions](https://peps.python.org/pep-0257/) — the canonical specification: triple-quoted strings, imperative-mood summary line ending in a period, blank line separating summary from detail, distinct rules for modules / classes / functions. Backs the chapter's "imperative summary" rule and the structural conventions.
- [PEP 8 — Style Guide for Python Code (Comments section)](https://peps.python.org/pep-0008/#comments) — comments should be complete sentences, in English, contradict the code only when the code is provably wrong; backs the comment-voice rules.
- [Google Python Style Guide § 3.8 Comments and Docstrings](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings) — when a docstring is required (public API, non-trivial size, non-obvious logic), the `Args` / `Returns` / `Raises` section format, and the rule that docstrings document usage rather than implementation. Source of the "skip docstrings on private trivial functions" rule.
- [NumPy docstring standard](https://numpydoc.readthedocs.io/en/latest/format.html) — alternative structured-section style supported by Sphinx via `napoleon`.
- [Sphinx napoleon extension](https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html) — confirms both Google and NumPy styles parse to the same generated reference.

### TypeScript / JavaScript

- [TSDoc specification (microsoft/tsdoc)](https://tsdoc.org/) — defines the standardized tag set for TypeScript doc comments and resolves JSDoc ambiguities. Source of the `@public` / `@internal` / `@beta` / `@alpha` API-stability tags.
- [microsoft/tsdoc on GitHub](https://github.com/microsoft/tsdoc) — implementation reference; backs the parser-not-generator framing.
- [JSDoc reference](https://jsdoc.app/) — canonical tag list for JavaScript projects.
- [TypeScript Handbook — JSDoc supported types](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html) — backs "do not duplicate type information that TypeScript already expresses."
- [TypeDoc documentation](https://typedoc.org/) — what the chapter recommends as the generator; consumes TSDoc and JSDoc.

### Rust

- [The rustdoc Book](https://doc.rust-lang.org/rustdoc/) — `///` outer and `//!` inner doc comments, Markdown body, doc tests run by `cargo test`. Backs the doctest-as-CI claim.
- [Rust API Guidelines — Documentation](https://rust-lang.github.io/api-guidelines/documentation.html) — required sections (`# Examples`, `# Panics`, `# Errors`, `# Safety`); imperative summary; first-line conventions.
- [`#![warn(missing_docs)]` lint](https://doc.rust-lang.org/rustc/lints/listing/allowed-by-default.html#missing-docs) — backs the recommendation to adopt the lint at crate level.

### Go

- [Effective Go — Commentary](https://go.dev/doc/effective_go#commentary) — godoc conventions: comments directly precede declarations, first sentence repeats the identifier name and is consumed as the summary.
- [Go Doc Comments — Andrew Gerrand and Russ Cox](https://go.dev/doc/comment) — full convention including links, lists, headings, and the rule that package comments live on `doc.go`.
- [pkg.go.dev rendering](https://pkg.go.dev/) — confirms the godoc convention as the surfaced public reference format.

### Other languages

- [YARD documentation](https://yardoc.org/) — Ruby tag conventions (`@param`, `@return`).
- [Javadoc Tool guide](https://docs.oracle.com/en/java/javase/21/docs/specs/javadoc/doc-comment-spec.html) — Java tag specification including first-sentence-as-summary and `@param` / `@throws` rules.
- [C# XML documentation comments (Microsoft Learn)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/xmldoc/) — `<summary>` / `<param>` / `<returns>` / `<exception>` schema.
- [Doxygen Manual](https://www.doxygen.nl/manual/) — C/C++/Objective-C convention.
- [DocFX](https://dotnet.github.io/docfx/) — what consumes C# XML doc comments into a hosted site.

## Reasoning and philosophy

- ["Code Tells You How, Comments Tell You Why" — Jeff Atwood (Coding Horror)](https://blog.codinghorror.com/code-tells-you-how-comments-tell-you-why/) — the canonical statement of the why-not-what rule the chapter adopts.
- [_Clean Code_ — Robert C. Martin, Chapter 4 "Comments"](https://www.oreilly.com/library/view/clean-code-a/9780136083238/) — argues that most comments are failures of expressive code; backs the "skip when the name does the work" rule.
- ["The Best Comments Are Those You Don't Have to Write" — Eric Lippert](https://ericlippert.com/2014/02/03/we-dont-need-comments/) — backs the rule against narrating-comments.
- [The Pragmatic Programmer (Hunt & Thomas) § "Documentation"](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/) — the docstring-as-contract framing.
- [Linus Torvalds on commenting (Linux kernel coding style)](https://www.kernel.org/doc/html/v6.0/process/coding-style.html#commenting) — "Generally, you want your comments to tell what your code does, not how"; the kernel-specific qualifier explained inline.

## Linting and tooling

- [`pydocstyle`](http://www.pydocstyle.org/) and [`ruff` (`D` rules)](https://docs.astral.sh/ruff/rules/#pydocstyle-d) — PEP 257 conformance.
- [`eslint-plugin-jsdoc`](https://github.com/gajus/eslint-plugin-jsdoc) — JSDoc/TSDoc lint rules including require-jsdoc, no-types-on-typed-params, valid-tags.
- [`tsdoc-config`](https://www.npmjs.com/package/@microsoft/tsdoc-config) — schema enforcement for TSDoc tag sets across a monorepo.
- [`clippy::missing_docs_in_private_items`](https://rust-lang.github.io/rust-clippy/master/index.html#missing_docs_in_private_items) — Rust lint reference.
- [`golint` / `revive`](https://github.com/mgechev/revive) — Go convention enforcement.
- [`pyright` / `mypy`](https://github.com/microsoft/pyright) — surface missing-docstring warnings via plugin configuration.

## Curated examples

Codebases worth reading for inline-doc style:

- [Python `cpython/Lib`](https://github.com/python/cpython/tree/main/Lib) — large-corpus PEP 257 in production.
- [Rust `std`](https://doc.rust-lang.org/std/) — exemplary rustdoc with full `# Examples` / `# Panics` / `# Errors` discipline.
- [Go `net/http`](https://pkg.go.dev/net/http) — godoc convention applied at the standard-library scale.
- [TypeScript `vscode`](https://github.com/microsoft/vscode) — TSDoc adopted across a large codebase; demonstrates `@public` / `@internal` partitioning.
- [Django source](https://github.com/django/django) — Google-style Python docstrings consumed by Sphinx.

## Uncited / heuristic claims to verify

- The "yearly audit of `@deprecated` tags" cadence is editorial.
- The framing that comments document _why_ and code documents _what_ is folk wisdom backed by Atwood, Martin, and the kernel coding style; no single canonical source defines it.
- The "wall of identical docstrings trains readers to ignore docstrings" claim is editorial, drawn from observed practice, not a single cited study.
- The "AI fingerprint vocabulary" is cross-referenced from `front-end/brand-and-copywriting.md`; this chapter does not duplicate the full list.

## Cross-references

- Generated reference output that consumes these comments → [`docs-site-structure.md`](docs-site-structure.md) § Reference.
- Voice rules for technical writing more broadly → `copywriting/technical-writing.md`.
- Voice and AI-fingerprint vocabulary → `front-end/brand-and-copywriting.md`.
- Code-style and lint integration → `front-end/code-style-and-quality.md` (when the rule overlaps with formatter behavior).
