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
  CTAs, SEO, structured data) read `front-end/brand-and-copywriting.md`
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
- AI copy DNA scan from `front-end/brand-and-copywriting.md` § Quick checklist before submitting.
- Cite human sources only.

## Task-to-chapter map

| Task                                                 | Primary                              | Also                                                                         |
| ---------------------------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| Any prose task — start here                          | `foundations.md`                     | `editing-and-anti-patterns.md`                                               |
| Formal vs informal, contractions, vocabulary tier    | `register-and-formality.md`          | `foundations.md`                                                             |
| Pick POV; build persona or character voice           | `voice-and-perspective.md`           | `register-and-formality.md`, `blogs-and-articles.md`                         |
| Blog, essay, op-ed, newsletter, long-form            | `blogs-and-articles.md`              | `foundations.md`, `voice-and-perspective.md`, `editing-and-anti-patterns.md` |
| Marketing: ads, emails, scripts, brochures, OOH      | `websites-and-marketing.md`          | `front-end/brand-and-copywriting.md`                                         |
| Web page copy (hero, pricing, microcopy, CTAs, SEO)  | `front-end/brand-and-copywriting.md` | `websites-and-marketing.md`                                                  |
| Tutorial, how-to, API reference, technical explainer | `technical-writing.md`               | `foundations.md`, `documentation/readme-guide.md`                            |
| README or project doc                                | `documentation/readme-guide.md`      | `technical-writing.md`                                                       |
| Research paper, abstract, white paper, lit review    | `research-and-academic.md`           | `foundations.md`, `technical-writing.md`                                     |
| Edit any draft; pre-publish review                   | `editing-and-anti-patterns.md`       | `front-end/brand-and-copywriting.md` § AI copy DNA                           |

## Domain checklists

Run before submitting:

- Register match — `register-and-formality.md` § Register markers.
- Voice consistency — `voice-and-perspective.md` § Voice consistency check.
- Genre fit — primary chapter for the genre.
- Five-pass edit — `editing-and-anti-patterns.md` § Five-pass edit.
- AI DNA scan — `front-end/brand-and-copywriting.md` § Quick checklist.

## Running order

1. Brief — audience, purpose, genre, register, POV, length, deadline, voice references.
2. Outline — beats per genre chapter; one-sentence promise per section.
3. Draft fast; do not edit while drafting.
4. Edit in five passes (truth → specificity → rhythm → voice → cut).
5. AI DNA scan; read aloud.

## Context shape

Confirm before drafting; ask if missing. Audience, purpose, genre, register, POV, length and channel constraints, two or three voice references, hard constraints (house style, legal, locale, accessibility).

## Skill connections

- Web copy, microcopy, hero/pricing/CTA, SEO → `front-end/brand-and-copywriting.md`.
- README, CONTRIBUTING, CHANGELOG, ARCHITECTURE → `documentation/AGENTS.md`.
- Localization register shifts → `front-end/internationalization.md`.
- Long-form content strategy, editorial calendars → external `content-strategy` skill if installed.
