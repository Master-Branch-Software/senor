---
name: documentation
description: |
  Guidelines for writing project documentation — READMEs, contributing guides,
  changelogs, architecture docs, API docs. Use this skill whenever the user
  asks to write, audit, or rewrite documentation files at the root of a
  repository or under `docs/`. Even if the user does not say "documentation"
  explicitly — if the task produces or edits a `README`, `CONTRIBUTING`,
  `CHANGELOG`, `ARCHITECTURE`, or any prose that ships with code, this skill
  applies.
---

# Documentation Guide

General operating principles live in `SKILL.md` at the repository root. Read it first.

## Non-negotiables

Apply these on every task without asking:

- A repository without a README is broken. If one is missing, write one before doing anything else.
- README lives at the repo root as `README.md` (English) or `README.<BCP-47>.md` (translations). One README per repo.
- Title, one-line description, install, usage. These four are not optional unless the project is a documentation-only repo.
- Every code block declares its language for syntax highlighting.
- Every install/usage command is copy-paste runnable. No placeholders that fail when pasted verbatim.
- License section is present and named. Link to the actual `LICENSE` file.
- No AI fingerprints in prose: no "delve," "leverage," "robust," "seamless," "unlock," em-dashes used as commas, or marketing-deck hedging.

## Task-to-chapter map

| Task                                                 | Primary                                | Also consider |
| ---------------------------------------------------- | -------------------------------------- | ------------- |
| Write a README from scratch                          | `readme-guide.md`                      | —             |
| Audit / rewrite an existing README                   | `readme-guide.md`                      | —             |
| Decide what belongs in README vs `docs/`             | `readme-guide.md` § Length and scope   | —             |
| Pick badges, screenshots, or GIFs for a project page | `readme-guide.md` § Visuals and badges | —             |

## Domain checklists

Run these before submitting any artifact:

- README pre-publish checklist — `readme-guide.md` § Pre-publish checklist. Address every miss.

## Running order for a new README

1. Brief — confirm: project name, one-sentence purpose, primary audience (end-user, integrator, contributor), install method, license.
2. Draft — title, one-liner, install, minimal usage example. Stop and confirm the example runs.
3. Expand — description, visuals (only if they earn their bytes), configuration, project status.
4. Contribution surface — contributing pointer, maintainers, license. Link out rather than inline long policies.
5. Self-review — run the pre-publish checklist in `readme-guide.md`.

## Skill connections

When a more specialized skill is installed, consult it alongside this one:

- Front-end / web copy voice — `front-end/brand-and-copywriting.md` for tone and AI-fingerprint avoidance.
- Architecture write-ups longer than a README section — `architecture/AGENTS.md` (when populated).
