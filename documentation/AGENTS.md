# Documentation Guide

General operating principles live in `SKILL.md` at the repository root. Read it first.

## Non-negotiables

- A repo without a README is broken. Write one before any other doc work.
- Every code block declares its language. Every install/usage command runs as pasted on a clean checkout.
- Docstrings document _how to use_ a symbol. Comments document _why_ this code exists. Do not collapse the two.
- No AI fingerprints in any artifact (`delve`, `leverage`, `seamless`, `robust`, marketing hedging, em-dashes as commas). Cross-reference the full list in `front-end/brand-and-copywriting.md` § Quick checklist.
- No placeholder text shipped (`[YOUR EMAIL]`, `<!-- TODO -->`, `lorem ipsum`).

## Task-to-chapter map

| Task                                                                                                                             | Primary                  | Also                                 |
| -------------------------------------------------------------------------------------------------------------------------------- | ------------------------ | ------------------------------------ |
| README — write, audit, scope vs `docs/`                                                                                          | `readme-guide.md`        | —                                    |
| `CONTRIBUTING.md`, commit conventions, PR/review rules, DCO/CLA                                                                  | `contributing-guide.md`  | `repo-meta-files.md`                 |
| `CHANGELOG.md`, release notes, semver coupling, generation tooling                                                               | `changelog-guide.md`     | `contributing-guide.md`              |
| `LICENSE`, `CODE_OF_CONDUCT`, `SECURITY`, `SUPPORT`, `GOVERNANCE`, `FUNDING`, `CODEOWNERS`, issue / PR templates, `CITATION.cff` | `repo-meta-files.md`     | —                                    |
| `docs/` directory or dedicated docs site (Diátaxis, framework, versioning, search)                                               | `docs-site-structure.md` | `copywriting/technical-writing.md`   |
| Docstrings, in-source comments, generated reference                                                                              | `inline-code-docs.md`    | `docs-site-structure.md` (Reference) |

## Domain checklists

Run before submitting; pointers to chapter sections:

- README pre-publish — `readme-guide.md` § Pre-publish checklist.
- CONTRIBUTING pre-publish — `contributing-guide.md` § Pre-publish checklist.
- Changelog pre-release — `changelog-guide.md` § Pre-release checklist.
- Repo meta files (per file) — `repo-meta-files.md` § Pre-publish checklist.
- Docs site pre-publish — `docs-site-structure.md` § Pre-publish checklist.
- Inline code docs pre-commit — `inline-code-docs.md` § Pre-commit checklist.

## Running order

1. Brief — confirm artifact, audience, scope. No brief, no draft.
2. Read the primary chapter; read "Also" only if the task crosses domains.
3. Draft to the chapter's required structure. Stop before extending; chapters define the order for a reason.
4. Verify — paste every command into a clean shell; check every link; run the chapter's checklist.
5. Self-review — name what was cut and what the reader will want next that you deliberately did not write.

## Skill connections

- Voice, Diátaxis, code-sample standards, instructions/warnings → `copywriting/technical-writing.md`.
- AI-fingerprint vocabulary, web copy voice → `front-end/brand-and-copywriting.md`.
- Architecture write-ups beyond a README section → `architecture/AGENTS.md` (when populated).
- Security policy implementation, threat models → `security/AGENTS.md` (when populated).
- Translated docs sites → `front-end/internationalization.md`.
