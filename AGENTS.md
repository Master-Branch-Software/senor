# Agents

Navigation index for any LLM or agent using this guide. Read this file first, then the chapters relevant to the current task. Do not ask the user to confirm rules that the chapters already define; apply them silently.

## Scope

- Repository root: the directory this file lives in.
- Chapters: `01` through `16`.
- Cheat sheet: `SUMMARY.md` (load when context is tight).
- External links: `LINKS.md`.
- Integration recipes for other tools: `USAGE.md`.

## Non-negotiables

See the `Non-negotiables` section of `README.md`. They apply on every task without asking.

## Task to chapters

Pick the row that matches the user intent. Read the primary chapter in full, then skim the related ones as needed.

- Build a new website from scratch — run the staged workflow in this file; read 15 first, then 01, 02, 03, 04, 05, 06, 07, 08, 11, 12, 13, 14, 16.
- Revise copy, voice, microcopy, SEO, structured data — 02, 15.
- Audit or improve accessibility — 05, 12, 09.
- Audit or improve performance (LCP, INP, CLS) — 06, 04, 07, 11, 08.
- Harden security or respond to an incident — 14, 08, 16, 07.
- Add or expand internationalization — 13, 02, 04, 05.
- Pick or refactor a stack, database, or API style — 08, 16, 15.
- Set up or strengthen tests — 12, 16.
- Add a new component or pattern — 08, 05, 07, 04, 16.
- Design tokens, colors, typography, dark mode — 03, 04, 05.
- Choose a browser support matrix or polyfill a feature — 11, 06.
- Pre-merge review or code review — 09, plus any chapter the diff touches.
- Launch or pre-deploy checklist — 14, 06, 05, 09, 16.
- Fix a bug or address a review comment — the subject-matter chapter, plus 09 for process.
- Discovery, scoping, staged asks, red-flag phrases — 15.

## Chapter relationships

Changes in one chapter usually affect its neighbours. Use this graph to decide what else to re-read when a recommendation spans concerns.

- 02 copy — 13 translations, 15 discovery, 09 review.
- 03 visual — 04 CSS, 05 contrast, 11 feature support.
- 04 CSS — 03, 06, 11, 16 CSS code style.
- 05 a11y — 07 semantic HTML, 12 a11y tests, 13 `lang`.
- 06 perf — 04, 07, 11, 08 hosting.
- 07 JS/HTML — 05, 11, 14 XSS, 16 code style.
- 08 stack — 12 testing, 14 security, 16 deps and tooling.
- 09 — review lens on every chapter.
- 11 browser compat — 04, 06, 07.
- 12 testing — 05, 09, 16 CI.
- 13 i18n — 02, 04, 05.
- 14 security — 07, 08, 16.
- 15 discovery — every chapter.
- 16 code style — 04, 07, 08, 12, 14.

## Operating principles

- Read the chapter before proposing a solution that touches it.
- Apply rules silently; do not paraphrase them back into prompt output.
- Ask the user only when the answer changes the output (per chapter 15).
- Produce written artifacts before code. Text reveals unclear thinking; code hides it.
- End every artifact with a one-line self-review naming any deviation from the guide.
- A later stage never overwrites an earlier artifact silently. If a later stage surfaces a flaw upstream, fix the upstream artifact first.

## Running order for a new website

Follow these stages when starting from scratch. Each stage produces a named artifact and pauses for user review.

1. Discovery — `brief.md`, `constraints.md`. Chapter 15. Captures audience, primary action, differentiator, target feel, success metric, deadline, browser matrix, locales, compliance baseline, and existing assets.
2. Voice — `brand.md`. Chapter 02. Archetype, voice attributes with "but not" modifiers, tone map, word lists.
3. Architecture — `sitemap.md` and `wireframes/<page>.md`. Chapters 02, 03, 15. Pages, primary goals, evidence types, section-level structure.
4. Copy — `copy/<page>.md`. Chapter 02, voiced by `brand.md`. Hero, value proposition, sections, microcopy, CTAs.
5. Tokens — `tokens.css`. Chapters 03, 04, 05, 08. OKLCH scales, type scale, spacing, radius, shadow; every foreground/background pair audited against WCAG 2.2 AA; tiered primitive → semantic → component.
6. Stack — `stack.md`. Chapters 08, 16. Framework, styling, primitives, image pipeline, hosting, package manager, runtime pin, formatter, linters, test runner, hook runner, dependency automation.
7. Scaffold — running repository. Chapters 04, 05, 07, 08, 12, 14, 16. Repository hygiene, code-quality toolchain, design-system foundation, security headers, i18n, CI pipeline.
8. Pages — implementation. Chapters 04, 05, 06, 07, 08, 09. Primitives, tokens, semantic HTML, responsive layout, accessibility defaults, per-change Conventional Commits.
9. Testing — `testing.md` and suites. Chapter 12. Vitest, Testing Library, MSW, Playwright, axe, visual regression.
10. Audits — accessibility (5), performance (6), security (14). Report violations with counts and severities; propose fixes ordered by impact; apply after user approval.
11. Launch — `launch.md`. Chapters 06, 09, 14, 16. Staging deploy, smoke tests, RUM, error tracking, structured logs, rollback, 30/90-day review against the success metric.

## Context shape (for future automation)

When the user or a front-end web app supplies answers, the context handed to the LLM should carry at least:

- Audience (one sentence, job-to-be-done).
- Primary action (one verb).
- Differentiator (grounded in truth).
- Target feel (one adjective).
- Success metric.
- Deadline and scope bounds.
- Browser support matrix (chapter 11 tier or explicit list).
- Locales in scope and RTL requirement (chapter 13).
- Compliance and security baseline (chapter 14: SOC 2, HIPAA, GDPR, PCI, none).
- Existing brand assets, content status, technical constraints.
- Stack preferences or hard constraints (chapter 08 recipe if selected).

An agent can ask for any missing item before starting the stages.
