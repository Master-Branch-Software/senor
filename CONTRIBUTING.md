# Contributing

This repository is a library of AI-readable skills — structured guidelines that make LLM-assisted coding more reliable, more human, and harder to get wrong. This document explains the structure so you can add or improve content without breaking how agents consume it.

## Repository structure

```
SKILL.md               Cross-domain operating rules (read by AI agents before any domain skill).
AGENTS.md              Developer rules for working in this repo (read this file and AGENTS.md first).
CONTRIBUTING.md        This file.

<domain>/
  AGENTS.md            Skill entry point — task routing, non-negotiables, running order.
  references/
    NN-topic.md        Chapter file — the actual guidelines (NN = two-digit number, e.g. 02).
    NN-topic.sources.md  Citations backing that chapter (developer artifact, not loaded by agents).
    SUMMARY.md         Condensed single-file reference for when context is tight.
  evals/               Eval prompt definitions.
  workspace/           Recorded eval runs (never formatted; in .prettierignore).
```

Current domains: `front-end/`, `security/`, `ruby/`, `architecture/`.

## Two file audiences

Every file in this repo is written for one of two audiences. Keep them separate.

| Audience | Files | Purpose |
| --- | --- | --- |
| **Skill consumers** — AI agents helping users | `SKILL.md`, `<domain>/AGENTS.md`, `references/NN-topic.md`, `references/SUMMARY.md` | Loaded at task time to guide agent behavior |
| **Developers** — humans and agents working in this repo | `AGENTS.md`, `references/NN-topic.sources.md`, `CONTRIBUTING.md` | Editorial and process tooling; never loaded by consumers |

## Branching model

- `master` — production. Merges here only from `development` via a maintainer PR. Never commit directly.
- `development` — the working branch. All PRs target this branch.

To contribute:

1. Fork the repository.
2. Create a feature branch from `development` in your fork (`git checkout -b my-change development`).
3. Make your changes and push to your fork.
4. Open a pull request from your fork's feature branch into `development` on the main repo.

Maintainers periodically merge `development` → `master` when the branch is stable.

## Adding content to an existing domain

1. **Find the right chapter.** Check `<domain>/AGENTS.md` for the task-to-chapter map. Extend an existing chapter before creating a new one.
2. **Edit the chapter file.** Follow the voice in `AGENTS.md` rule 6: terse, imperative, no hedging. One claim, one sentence.
3. **Add a citation.** Every new or changed rule needs a source in the matching `NN-topic.sources.md`. Human-authored sources only — official specs, MDN, established engineering blogs, production codebases, books. No AI-generated content.
4. **Update `SUMMARY.md`.** If the change affects a summary bullet, update it in the same commit.
5. **Run an eval.** New or significantly changed rules belong in `workspace/iteration-N/` as a WITH-skill vs WITHOUT-skill comparison. Guidance that doesn't change agent behavior is dead weight.
6. **Run Prettier.** `npx prettier --write <file>` on every file you touch except those in `.prettierignore`.

## Adding a new domain

1. Create `<domain>/AGENTS.md` using the template below.
2. Create `<domain>/references/` when you have at least one chapter to add.
3. Add the domain to the routing table in root `SKILL.md`.
4. Add the domain to the project layout in root `AGENTS.md`.
5. Add any eval-output directories to `.prettierignore`.

### Domain AGENTS.md template

```markdown
---
name: <domain>
description: |
  One to three sentences describing when an AI agent should load this skill.
  Be specific — the description is used by agents to decide relevance.
---

# <Domain> Guide

General operating principles live in `SKILL.md` at the repository root. Read it first.

## Non-negotiables

Apply these on every task without asking:

- [list the absolute minimums for this domain]

## Task-to-chapter map

| Task | Primary | Also consider |
| --- | --- | --- |
| [task] | `NN-topic.md` | `NN-topic.md` |

## Domain checklists

Run these before submitting any artifact:

- [checklist item referencing a specific section in a chapter file]

## Running order for [domain task type]

1. [stage] — [artifact]. [What to produce and what to confirm.]

## Skill connections

When a more specialized skill is installed, consult it alongside this one:

- [related skill] → [when to reach for it]
```

## Adding a chapter file

Chapter files use a two-digit prefix (`NN-topic.md`) so they sort consistently and agents can reference them by number. The next available number in a domain is `max(existing NN) + 1`.

Chapter voice rules (from `AGENTS.md`):

- Terse, imperative, no hedging. "Two-space indentation." not "Generally prefer two spaces."
- No emojis. No conversational filler.
- Numbered chapter files are curated. New chapters require user (maintainer) approval.
- Compress for LLM reading — every saved token is leverage at inference time.

## Eval convention

Evals live in `<domain>/workspace/iteration-N/`. Each eval compares WITH-skill vs WITHOUT-skill agent output on a representative task. The goal is to prove that a rule change actually changes agent behavior — not just reads well.

Record format: raw agent output, unformatted, with a `summary.md` describing what changed and why.

## What not to do

- Do not create a new chapter when an existing one can be extended.
- Do not write AI-generated content as a source citation.
- Do not add a rule without a citation in the matching `*.sources.md`.
- Do not format files in `.prettierignore` — they are intentionally hand-formatted or artifact data.
- Do not edit `SKILL.md` (root) or `AGENTS.md` (root) for domain-specific content — those are cross-cutting files.
