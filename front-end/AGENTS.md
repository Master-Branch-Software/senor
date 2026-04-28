---
name: front-end
description: |
  Professional-grade web design, frontend engineering, and copywriting guidelines.
  Use this skill whenever the user asks about websites, landing pages, web apps,
  UI components, CSS, layout, responsive design, accessibility, performance,
  Core Web Vitals, color palettes, typography, design tokens, brand voice,
  copywriting, microcopy, SEO, structured data, web security headers,
  browser compatibility, testing strategies, internationalization, or stack selection.
  Also use for: reviewing web code, picking frameworks, scaffolding repos,
  writing HTML/CSS/JS/TS, choosing hosting, or auditing an existing site.
  Even if the user does not say "design" or "website" explicitly — if the task
  produces or modifies anything rendered in a browser, this skill applies.
  When a more specialized skill is installed (security, devops, database-design),
  read it too for deeper coverage of that subdomain. For prose work that goes
  beyond web pages — long-form blogs, ad copy, technical writing, research,
  formality registers, narrative voice — also load `copywriting/AGENTS.md`.
---

# Front-end Guide

General operating principles live in `SKILL.md` at the repository root. Read it first.

## Non-negotiables

Apply these on every task without asking:

- Every `<img>` has `width`, `height`, and meaningful `alt`.
- The likely LCP image is never lazy-loaded. Use `fetchpriority="high"` on the `<img>` and preload it when the browser would otherwise discover it late.
- Body text ≥ 16 px, line-height 1.5, line length 60–75 characters.
- WCAG 2.2 AA contrast: 4.5:1 body text, 3:1 large text, 3:1 UI and focus rings.
- Visible `:focus-visible` on every focusable element.
- `prefers-reduced-motion` is honored.
- Native HTML first: `<button>`, `<dialog>`, `<details>`, `popover` attribute before custom.
- Fonts use `font-display: swap` with matched-metric fallbacks or `font-display: optional`.
- No positive `tabindex`.
- Two-space indent, kebab-case files, `visible` not `isVisible`, logical properties, `.DS_Store` in `.gitignore`.

## Task-to-chapter map

Match user intent to the primary reference file. Read it in full, then skim related.

| Task                                                                   | Primary                                                                                                          | Also consider                                                                                          |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| New website from scratch                                               | `discovery-and-communication.md` then every other chapter except `inspiration-gallery.md`                        | —                                                                                                      |
| Revise copy, voice, microcopy, SEO, structured data                    | `brand-and-copywriting.md`                                                                                       | `discovery-and-communication.md`, `anti-patterns-and-process.md`                                       |
| Audit/improve accessibility                                            | `accessibility.md`                                                                                               | `testing.md`, `anti-patterns-and-process.md`                                                           |
| Audit/improve performance (LCP, INP, CLS)                              | `performance.md`                                                                                                 | `css-and-layout.md`, `javascript-and-html.md`, `browser-compatibility.md`, `stack-and-architecture.md` |
| Harden security or respond to incident                                 | `security.md`                                                                                                    | `stack-and-architecture.md`, `code-style-and-quality.md`, `javascript-and-html.md`                     |
| Add/expand internationalization                                        | `internationalization.md`                                                                                        | `brand-and-copywriting.md`, `css-and-layout.md`, `accessibility.md`                                    |
| Pick/refactor stack, database, API style                               | `stack-and-architecture.md`                                                                                      | `code-style-and-quality.md`, `discovery-and-communication.md`                                          |
| Set up/strengthen tests                                                | `testing.md`                                                                                                     | `code-style-and-quality.md`                                                                            |
| Add component or pattern                                               | `stack-and-architecture.md`                                                                                      | `accessibility.md`, `javascript-and-html.md`, `css-and-layout.md`, `code-style-and-quality.md`         |
| Design tokens, colors, typography, dark mode                           | `visual-design.md`                                                                                               | `css-and-layout.md`, `accessibility.md`                                                                |
| Layout archetype, hero pattern, section flow                           | `visual-design.md`                                                                                               | `css-and-layout.md`, `inspiration-gallery.md`                                                          |
| Need creative reference / browse examples by feel                      | `inspiration-gallery.md`                                                                                         | `visual-design.md`                                                                                     |
| Avoid AI design fingerprints (purple gradients, generic SaaS template) | `anti-patterns-and-process.md` § AI design fingerprints                                                          | `visual-design.md` § Designing against AI defaults                                                     |
| Browser support matrix or polyfill                                     | `browser-compatibility.md`                                                                                       | `performance.md`                                                                                       |
| Pre-merge/code review                                                  | `anti-patterns-and-process.md`                                                                                   | Any chapters the diff touches                                                                          |
| Launch/pre-deploy checklist                                            | `security.md`, `performance.md`, `accessibility.md`, `anti-patterns-and-process.md`, `code-style-and-quality.md` | —                                                                                                      |
| Bug fix or review comment                                              | Subject-matter chapter + `anti-patterns-and-process.md`                                                          | —                                                                                                      |
| Discovery, scoping, staged asks, red-flag phrases                      | `discovery-and-communication.md`                                                                                 | —                                                                                                      |

## Domain checklists

Run these before submitting any artifact — not as a read, but as a scan of work already produced.

- **Copy:** 14-item AI copy DNA quick checklist — `brand-and-copywriting.md` § Quick checklist for any draft. Address every hit.
- **Design:** AI design fingerprints audit — `anti-patterns-and-process.md` § AI design fingerprints. Remove or justify every match.

## Running order for a new website

Each stage produces a named artifact; pause for user review.

1. Discovery — `brief.md`, `constraints.md`. Audience, primary action, differentiator, feel, metric, deadline, browser matrix, locales, compliance, assets.
2. Voice — `brand.md`. Archetype, voice with "but not" modifiers, tone map, word lists.
3. Architecture — `sitemap.md` + `wireframes/<page>.md`. Pages, goals, evidence types, section structure.
4. Copy — `copy/<page>.md`. Hero, value prop, sections, microcopy, CTAs. Voiced by `brand.md`.
5. Tokens — `tokens.css`. OKLCH scales, type, spacing, radius, shadow; every fg/bg pair audited WCAG 2.2 AA; primitive → semantic → component.
6. Stack — `stack.md`. Framework, styling, primitives, image pipeline, hosting, pkg manager, runtime pin, formatter, linters, test runner, hooks, dep automation.
7. Scaffold — running repo. Repo hygiene, code-quality toolchain, design-system foundation, security headers, i18n, CI.
8. Pages — implementation. Primitives, tokens, semantic HTML, responsive layout, a11y defaults, Conventional Commits per change.
9. Testing — `testing.md` + suites. Vitest, Testing Library, MSW, Playwright, axe, visual regression.
10. Audits — a11y (`accessibility.md`), perf (`performance.md`), security (`security.md`). Report violations with counts + severities; fixes ordered by impact; apply after approval.
11. Launch — `launch.md`. Staging deploy, smoke tests, RUM, error tracking, structured logs, rollback, 30/90-day review vs success metric.

## Context shape

Before starting stages, confirm at minimum:

- Audience (one sentence, job-to-be-done).
- Primary action (one verb).
- Differentiator (grounded in truth).
- Target feel (one adjective).
- Success metric.
- Deadline + scope bounds.
- Browser matrix (`browser-compatibility.md` tier or explicit list).
- Locales + RTL requirement (`internationalization.md`).
- Compliance baseline (`security.md` covers SOC 2, HIPAA, GDPR, PCI, none).
- Brand assets, content status, technical constraints.
- Stack preferences or hard constraints (`stack-and-architecture.md` recipe if selected).

Ask for missing items before starting stages.

## Skill connections

When a more specialized skill is installed, consult it alongside this one:

- Deep security audits, pen-testing, threat modeling → `security/AGENTS.md`.
- Infrastructure provisioning, CI/CD pipelines, container orchestration → `devops`, `cloud-deploy`, or `platform` skill.
- Database schema design, query optimization, migrations → `database-design` or `sql` skill.
- Cross-genre prose — long-form blogs, ad copy, scripts, technical writing, research, formality registers, narrative voice → `copywriting/AGENTS.md`.
- Long-form content strategy, editorial calendars, SEO beyond structured data → external `content-strategy` or `seo` skill if installed.
