---
name: web-design
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
  When a more specialized skill is installed (security, devops, database-design,
  copywriting), read it too for deeper coverage of that subdomain.
---

# Web Design Guide

A reference for building professional-grade websites in LLM-assisted workflows.
Load this skill at the start of any session that will produce or review web code,
copy, or visual design.

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

| Task | Primary | Also consider |
|---|---|---|
| New website from scratch | `15-discovery-and-communication.md` then `01`, `02`, `03`, `04`, `05`, `06`, `07`, `08`, `11`, `12`, `13`, `14`, `16` | `SUMMARY.md` for quick refresher |
| Revise copy, voice, microcopy, SEO, structured data | `02-brand-and-copywriting.md` | `15-discovery-and-communication.md`, `09-anti-patterns-and-process.md` |
| Audit/improve accessibility | `05-accessibility.md` | `12-testing.md`, `09-anti-patterns-and-process.md` |
| Audit/improve performance (LCP, INP, CLS) | `06-performance.md` | `04-css-and-layout.md`, `07-javascript-and-html.md`, `11-browser-compatibility.md`, `08-stack-and-architecture.md` |
| Harden security or respond to incident | `14-security.md` | `08-stack-and-architecture.md`, `16-code-style-and-quality.md`, `07-javascript-and-html.md` |
| Add/expand internationalization | `13-internationalization.md` | `02-brand-and-copywriting.md`, `04-css-and-layout.md`, `05-accessibility.md` |
| Pick/refactor stack, database, API style | `08-stack-and-architecture.md` | `16-code-style-and-quality.md`, `15-discovery-and-communication.md` |
| Set up/strengthen tests | `12-testing.md` | `16-code-style-and-quality.md` |
| Add component or pattern | `08-stack-and-architecture.md` | `05-accessibility.md`, `07-javascript-and-html.md`, `04-css-and-layout.md`, `16-code-style-and-quality.md` |
| Design tokens, colors, typography, dark mode | `03-visual-design.md` | `04-css-and-layout.md`, `05-accessibility.md` |
| Browser support matrix or polyfill | `11-browser-compatibility.md` | `06-performance.md` |
| Pre-merge/code review | `09-anti-patterns-and-process.md` | Any chapters the diff touches |
| Launch/pre-deploy checklist | `14-security.md`, `06-performance.md`, `05-accessibility.md`, `09-anti-patterns-and-process.md`, `16-code-style-and-quality.md` | `SUMMARY.md` |
| Bug fix or review comment | Subject-matter chapter + `09-anti-patterns-and-process.md` | — |
| Discovery, scoping, staged asks, red-flag phrases | `15-discovery-and-communication.md` | `SUMMARY.md` |

## Operating principles

1. Read the primary chapter before proposing any solution touching it.
2. Apply rules silently; never paraphrase back into output.
3. Ask only when the answer changes the output (`15-discovery-and-communication.md`).
4. Written artifacts before code. Text reveals unclear thinking; code hides it.
5. End every artifact with a one-line self-review naming deviations.
6. Later stage never silently overwrites earlier artifact. Upstream flaw → fix upstream first.

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
10. Audits — a11y (`05`), perf (`06`), security (`14`). Report violations with counts + severities; fixes ordered by impact; apply after approval.
11. Launch — `launch.md`. Staging deploy, smoke tests, RUM, error tracking, structured logs, rollback, 30/90-day review vs success metric.

## Context shape

Before starting stages, confirm at minimum:

- Audience (one sentence, job-to-be-done).
- Primary action (one verb).
- Differentiator (grounded in truth).
- Target feel (one adjective).
- Success metric.
- Deadline + scope bounds.
- Browser matrix (`11-browser-compatibility.md` tier or explicit list).
- Locales + RTL requirement (`13-internationalization.md`).
- Compliance baseline (`14-security.md`: SOC 2, HIPAA, GDPR, PCI, none).
- Brand assets, content status, technical constraints.
- Stack preferences or hard constraints (`08-stack-and-architecture.md` recipe if selected).

Ask for missing items before starting stages.

## Skill connections

When a more specialized skill is installed, consult it alongside this one:

- Deep security audits, pen-testing, threat modeling → `security`, `infosec`, or `appsec` skill.
- Infrastructure provisioning, CI/CD pipelines, container orchestration → `devops`, `cloud-deploy`, or `platform` skill.
- Database schema design, query optimization, migrations → `database-design` or `sql` skill.
- Long-form content strategy, editorial calendars, SEO beyond structured data → `copywriting`, `content-strategy`, or `seo` skill.

## How to read references

1. Start with this SKILL.md.
2. Load the primary chapter for the current task.
3. Load SUMMARY.md only when context is tight and you need a quick refresher.
4. Load USAGE.md when the user asks how to wire this guide into Warp, Cursor, Claude Projects, Copilot, or other tools.
5. Load LINKS.md when you need external reference URLs.
6. Do not load all reference files at once. Progressive disclosure keeps context usable.

## How to prompt an LLM using this guide

### One-prompt kickoff

```
Read web-design/SKILL.md and drive a new website project per its
"Running order for a new website". At each stage, ask me the questions
you need — one at a time — produce the named artifact, and pause for my
review. Ask only what changes the output. Do not restate the guide's
rules in your answers.
```

### Per-stage prompts

- Discovery: `Interview me for brief.md and constraints.md, one question at a time.`
- Voice: `Produce brand.md from brief.md.`
- Architecture: `Produce sitemap.md and wireframes/<page>.md.`
- Copy: `Write copy/<page>.md for each page in sitemap.md. Voice: brand.md.`
- Tokens: `Produce tokens.css with a WCAG 2.2 AA audit for every foreground/background pair.`
- Stack: `Produce stack.md. List exact packages and current versions.`
- Scaffold: `Scaffold the repository. Commit each sub-step separately.`
- Pages: `Implement /<page>. End with a self-review naming any deviation.`
- Testing: `Wire the testing harness. Output testing.md.`
- Accessibility audit: `Audit /<page>. Report violations with impact; apply after approval.`
- Performance audit: `Audit /<page>. Same reporting format.`
- Security audit: `Audit the app. Same reporting format.`
- Launch: `Prepare launch.md. Do not deploy until the pre-merge checklist and security baseline pass.`

### Targeted tasks

- `Revise the pricing-page copy. Voice: brand.md.`
- `Update security for this website.`
- `Improve LCP on /products/*.`
- `Add Dutch and French locales.`
- `Set up tests for the /<path> route.`
- `Add a Combobox component.`
- `Pick a browser support matrix for this project.`
- `Do a pre-merge review on this PR.`

## When in doubt

Favor the simplest thing that works, uses the platform, respects the user, and reads well six months from now.
