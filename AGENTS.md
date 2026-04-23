# Agents

Navigation index for LLMs. Read first, then pull chapters for current task. Apply rules silently; never ask user to confirm rules chapters already define.

## Scope

- Repo root: directory this file lives in.
- Chapters: `01`–`16`.
- Cheat sheet: `SUMMARY.md` (load when context tight).
- External links: `LINKS.md`.
- Integration recipes: `USAGE.md`.

## Non-negotiables

See `Non-negotiables` in `README.md`. Apply on every task without asking.

## Task to chapters

Match user intent to row. Read primary chapter in full, skim related.

- New website from scratch — staged workflow below; read 15 first, then 01, 02, 03, 04, 05, 06, 07, 08, 11, 12, 13, 14, 16.
- Revise copy, voice, microcopy, SEO, structured data — 02, 15.
- Audit/improve accessibility — 05, 12, 09.
- Audit/improve performance (LCP, INP, CLS) — 06, 04, 07, 11, 08.
- Harden security or respond to incident — 14, 08, 16, 07.
- Add/expand internationalization — 13, 02, 04, 05.
- Pick/refactor stack, database, API style — 08, 16, 15.
- Set up/strengthen tests — 12, 16.
- Add component or pattern — 08, 05, 07, 04, 16.
- Design tokens, colors, typography, dark mode — 03, 04, 05.
- Browser support matrix or polyfill — 11, 06.
- Pre-merge/code review — 09, plus chapters diff touches.
- Launch/pre-deploy checklist — 14, 06, 05, 09, 16.
- Bug fix or review comment — subject-matter chapter + 09.
- Discovery, scoping, staged asks, red-flag phrases — 15.

## Chapter relationships

- 02 copy — 13, 15, 09.
- 03 visual — 04, 05, 11.
- 04 CSS — 03, 06, 11, 16.
- 05 a11y — 07, 12, 13 `lang`.
- 06 perf — 04, 07, 11, 08.
- 07 JS/HTML — 05, 11, 14, 16.
- 08 stack — 12, 14, 16.
- 09 — review lens on every chapter.
- 11 browser compat — 04, 06, 07.
- 12 testing — 05, 09, 16.
- 13 i18n — 02, 04, 05.
- 14 security — 07, 08, 16.
- 15 discovery — every chapter.
- 16 code style — 04, 07, 08, 12, 14.

## Operating principles

- Read chapter before proposing solution touching it.
- Apply rules silently; never paraphrase back into output.
- Ask only when answer changes output (ch 15).
- Written artifacts before code. Text reveals unclear thinking; code hides it.
- End every artifact with one-line self-review naming deviations.
- Later stage never silently overwrites earlier artifact. Upstream flaw → fix upstream first.

## Running order for a new website

Each stage produces named artifact; pause for user review.

1. Discovery — `brief.md`, `constraints.md`. Ch 15. Audience, primary action, differentiator, feel, metric, deadline, browser matrix, locales, compliance, assets.
2. Voice — `brand.md`. Ch 02. Archetype, voice with "but not" modifiers, tone map, word lists.
3. Architecture — `sitemap.md` + `wireframes/<page>.md`. Ch 02, 03, 15. Pages, goals, evidence types, section structure.
4. Copy — `copy/<page>.md`. Ch 02, voiced by `brand.md`. Hero, value prop, sections, microcopy, CTAs.
5. Tokens — `tokens.css`. Ch 03, 04, 05, 08. OKLCH scales, type, spacing, radius, shadow; every fg/bg pair audited WCAG 2.2 AA; primitive → semantic → component.
6. Stack — `stack.md`. Ch 08, 16. Framework, styling, primitives, image pipeline, hosting, pkg manager, runtime pin, formatter, linters, test runner, hooks, dep automation.
7. Scaffold — running repo. Ch 04, 05, 07, 08, 12, 14, 16. Repo hygiene, code-quality toolchain, design-system foundation, security headers, i18n, CI.
8. Pages — implementation. Ch 04, 05, 06, 07, 08, 09. Primitives, tokens, semantic HTML, responsive layout, a11y defaults, Conventional Commits per change.
9. Testing — `testing.md` + suites. Ch 12. Vitest, Testing Library, MSW, Playwright, axe, visual regression.
10. Audits — a11y (05), perf (06), security (14). Report violations with counts + severities; fixes ordered by impact; apply after approval.
11. Launch — `launch.md`. Ch 06, 09, 14, 16. Staging deploy, smoke tests, RUM, error tracking, structured logs, rollback, 30/90-day review vs success metric.

## Context shape (for future automation)

LLM context needs at minimum:

- Audience (one sentence, job-to-be-done).
- Primary action (one verb).
- Differentiator (grounded in truth).
- Target feel (one adjective).
- Success metric.
- Deadline + scope bounds.
- Browser matrix (ch 11 tier or explicit list).
- Locales + RTL requirement (ch 13).
- Compliance baseline (ch 14: SOC 2, HIPAA, GDPR, PCI, none).
- Brand assets, content status, technical constraints.
- Stack preferences or hard constraints (ch 08 recipe if selected).

Ask for missing items before starting stages.
