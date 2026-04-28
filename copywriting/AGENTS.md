---
name: copywriting
description: |
  Cross-genre prose — formality registers, narrative voice and persona,
  long-form blogs, marketing copy beyond websites, technical writing,
  research and academic writing. Use whenever the task is to write,
  edit, or audit prose: blog posts, white papers, op-eds, newsletters,
  email campaigns, scripts, ad copy, tutorials, API guides, research
  papers, abstracts, case studies. Even when the user does not say
  "copywriting" — if the deliverable is words a human will read, this
  skill applies. For web-specific page patterns (hero, pricing, microcopy,
  CTAs, SEO, structured data) read `front-end/references/02-brand-and-copywriting.md`
  alongside this skill. For README and project-doc structure read
  `documentation/AGENTS.md`.
---

# Copywriting Guide

General operating principles live in `SKILL.md` at the repository root. Read it first.

## Non-negotiables

- Pick register before drafting (frozen / formal / consultative / casual / intimate). No register chosen → mid-formal AI mush.
- Pick POV before drafting (1st singular / 1st plural / 2nd / 3rd). Mid-piece switches are rewrites.
- Audience brief: who, what they know, what changes after reading. No brief, no draft.
- Active voice default; concrete over abstract; read aloud.
- AI copy DNA scan from `front-end/references/02-brand-and-copywriting.md` § Quick checklist before submitting.
- Cite human sources only.

## Task-to-chapter map

| Task                                                 | Primary                                            | Also                                                             |
| ---------------------------------------------------- | -------------------------------------------------- | ---------------------------------------------------------------- |
| Any prose task — start here                          | `01-foundations.md`                                | `08-editing-and-anti-patterns.md`                                |
| Formal vs informal, contractions, vocabulary tier    | `02-register-and-formality.md`                     | `01`                                                             |
| Pick POV; build persona or character voice           | `03-voice-and-perspective.md`                      | `02`, `04`                                                       |
| Blog, essay, op-ed, newsletter, long-form            | `04-blogs-and-articles.md`                         | `01`, `03`, `08`                                                 |
| Marketing: ads, emails, scripts, brochures, OOH      | `05-websites-and-marketing.md`                     | `front-end/references/02-brand-and-copywriting.md`               |
| Web page copy (hero, pricing, microcopy, CTAs, SEO)  | `front-end/references/02-brand-and-copywriting.md` | `05`                                                             |
| Tutorial, how-to, API reference, technical explainer | `06-technical-writing.md`                          | `01`, `documentation/references/01-readme.md`                    |
| README or project doc                                | `documentation/references/01-readme.md`            | `06`                                                             |
| Research paper, abstract, white paper, lit review    | `07-research-and-academic.md`                      | `01`, `06`                                                       |
| Edit any draft; pre-publish review                   | `08-editing-and-anti-patterns.md`                  | `front-end/references/02-brand-and-copywriting.md` § AI copy DNA |

## Domain checklists

Run before submitting:

- Register match — `02-register-and-formality.md` § Register markers.
- Voice consistency — `03-voice-and-perspective.md` § Voice consistency check.
- Genre fit — primary chapter for the genre.
- Five-pass edit — `08-editing-and-anti-patterns.md` § Five-pass edit.
- AI DNA scan — `front-end/references/02-brand-and-copywriting.md` § Quick checklist.

## Running order

1. Brief — audience, purpose, genre, register, POV, length, deadline, voice references.
2. Outline — beats per genre chapter; one-sentence promise per section.
3. Draft fast; do not edit while drafting.
4. Edit in five passes (truth → specificity → rhythm → voice → cut).
5. AI DNA scan; read aloud.

## Context shape

Confirm before drafting; ask if missing. Audience, purpose, genre, register, POV, length and channel constraints, two or three voice references, hard constraints (house style, legal, locale, accessibility).

## Skill connections

- Web copy, microcopy, hero/pricing/CTA, SEO → `front-end/references/02-brand-and-copywriting.md`.
- README, CONTRIBUTING, CHANGELOG, ARCHITECTURE → `documentation/AGENTS.md`.
- Localization register shifts → `front-end/references/12-internationalization.md`.
- Long-form content strategy, editorial calendars → external `content-strategy` skill if installed.
