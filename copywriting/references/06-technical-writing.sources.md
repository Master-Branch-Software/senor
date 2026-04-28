# Sources — 06 Technical Writing

## Primary citations

- Daniele Procida, "Diátaxis: A systematic approach to technical documentation authoring." <https://diataxis.fr/>. Source for the four document types (tutorial, how-to, reference, explanation) and the rule that mixing types confuses readers. Adopted by Canonical/Ubuntu, GitLab, Cloudflare, and others as the foundation for technical doc systems. Adoption write-up: <https://ubuntu.com/blog/diataxis-a-new-foundation-for-canonical-documentation>.
- Google developer documentation style guide. <https://developers.google.com/style>. Source for second-person, present-tense, active-voice as the technical-writing default. Public since 2017.
- Microsoft Writing Style Guide. <https://learn.microsoft.com/en-us/style-guide/welcome/>. Source for "write like you speak" and contraction usage in technical content.
- Write the Docs community style-guide directory. <https://www.writethedocs.org/guide/writing/style-guides/>. Comparative reference for technical writing conventions across organizations.

## Secondary references

- IBM Style Guide (Pearson, 2nd ed., 2018). Older but rigorous; particularly strong on warnings/cautions/notes conventions.
- Apple Style Guide. <https://help.apple.com/applestyleguide/>. Source for consumer-facing technical writing; useful contrast with Google/Microsoft developer-focused styles.
- Read Me Driven Development (Tom Preston-Werner). <https://tom.preston-werner.com/2010/08/23/readme-driven-development.html>. Source for the discipline of writing the README before the code; cross-reference with `documentation/references/01-readme.md`.
- Mike Pope, "Five Tips for Writing Great How-Tos." Industry blog post; useful checklist of how-to anti-patterns.

## Curated tool catalog

- Vale (vale.sh) — open-source prose linter; supports custom rule packs for Google, Microsoft, write-the-docs styles. Integrates with CI.
- alex.js — flags inconsiderate writing (gendered terms, ableist language) in docs.
- markdownlint — structural lint for Markdown docs (headings, lists, link formatting).
- Sphinx, MkDocs, Docusaurus — common documentation site generators with first-class support for Diátaxis-style structures.
- Doctest (Python) and similar — execute code samples as part of CI to prevent rot.

## Notes for editors

The Diátaxis taxonomy is treated as canonical in modern technical-documentation practice; cite Procida's diataxis.fr directly rather than secondary explanations.

The "common technical-writing failures" section overlaps with `08-editing-and-anti-patterns.md`. When extending, keep failure-mode descriptions specific to technical writing (encyclopedic tutorials, conditional sprawl, marketing voice in docs) rather than restating cross-genre antipatterns.

Code-sample testing is mandatory for any documentation that ships with software; the absence of tested samples is the largest single source of doc rot. Cite a CI integration tool when adding new code-sample standards.
