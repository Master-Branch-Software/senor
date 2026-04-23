# Web Design & Development Master Guide

A curated, opinionated reference for designing and building professional-grade websites. Written to be loaded at the start of any LLM session that will produce or review web code, copy, or visual design decisions.

## Purpose

The default output of AI systems tends toward junior-level code and generic design: media-query soup, hard-coded pixel values, unlabelled form fields, bland copy, low-contrast color, and motion that ignores user preferences. This guide encodes the practices that separate professional, maintainable, human-feeling websites from that default.

## How to use

1. Read the relevant chapter before touching code. If a task concerns color, read `03-visual-design.md`. If it concerns performance, read `06-performance.md`.
2. Apply stable requirements directly. For performance hints and newly available features, verify the discovery path, support matrix, and fallback strategy before implementation.
3. Prefer the documented pattern when it fits the page architecture and support matrix. Use `11-browser-compatibility.md` for guard, polyfill, and fallback decisions.
4. When a rule conflicts with a user requirement, follow the user. When a rule conflicts with a habit, follow the rule.

## Chapters

- [SUMMARY.md](SUMMARY.md) — Condensed cheat sheet of every chapter. Load this when context is tight.
- [AGENTS.md](AGENTS.md) — Navigation index for LLMs. Task-to-chapter map, chapter relationships, running order for new projects. Every agent should read this first.
- [USAGE.md](USAGE.md) — How to wire this guide into Warp, Cursor, Claude Projects, Copilot, and other tools; prompt patterns that work.
- [01-philosophy-and-psychology.md](01-philosophy-and-psychology.md) — Intent, craft, Laws of UX, Nielsen heuristics, Cialdini's 7 principles of persuasion, Gestalt principles, emotional design.
- [02-brand-and-copywriting.md](02-brand-and-copywriting.md) — Jungian brand archetypes, voice attributes with "but not" qualifiers, tone ladder by context, voice-of-customer research and language harvesting, copywriting frameworks (AIDA, PAS, BAB, 4Ps, FAB, StoryBrand) with worked examples, headline formulas, hero and value-proposition patterns, page-type copy skeletons (home, pricing, product, about, blog, contact, 404/500, legal, transactional email), microcopy, CTA psychology, social proof, navigation labels, notifications, cookie/consent banners, power words and specificity, reading-level targets, editing passes, copy measurement and testing, SEO in the AI-search era, JSON-LD structured data.
- [03-visual-design.md](03-visual-design.md) — Visual hierarchy, spacing scales, typography (scales, variable fonts, fluid), color systems (OKLCH, APCA, Radix, tooling), design styles.
- [04-css-and-layout.md](04-css-and-layout.md) — CSS units, responsive strategy, modern CSS (container queries, `:has()`, `@layer`, subgrid, view transitions, `@property`, `@function`, `if()`), motion principles.
- [05-accessibility.md](05-accessibility.md) — WCAG 2.2, semantic HTML, the five ARIA rules, keyboard navigation, focus management, screen reader testing, automated auditing.
- [06-performance.md](06-performance.md) — Core Web Vitals (LCP, INP, CLS), bfcache, image optimization, font loading, third-party script strategy, critical rendering path, RUM.
- [07-javascript-and-html.md](07-javascript-and-html.md) — ECMAScript 2025 and 2026, Temporal, iterator helpers, the Signals proposal, web components, runtime selection (Node/Deno/Bun), modern HTML primitives.
- [08-stack-and-architecture.md](08-stack-and-architecture.md) — Framework selection, project-type stack recipes, rendering strategies (SSG, SSR, ISR, streaming, CSR, edge, islands, resumability, React Server Components), API styles (REST + OpenAPI, tRPC, GraphQL, Server Actions, gRPC/Connect, WebSockets, SSE, webhooks), backend language choice, database and CMS and auth and hosting selection, Tailwind v4, shadcn/ui, design tokens, component architecture, scaling ladder, maintainability and the boring-technology budget.
- [09-anti-patterns-and-process.md](09-anti-patterns-and-process.md) — Common AI and junior-level mistakes to avoid, review checklist, design-and-ship workflow.
- [10-resources.md](10-resources.md) — Trusted external references, tools, calculators, and libraries.
- [11-browser-compatibility.md](11-browser-compatibility.md) — Choosing a support matrix, Baseline, `@supports` and feature detection, polyfills, per-feature status, cross-browser testing.
- [12-testing.md](12-testing.md) — The testing trophy, Vitest + Testing Library, MSW, Playwright E2E, visual regression, accessibility automation, CI integration.
- [13-internationalization.md](13-internationalization.md) — Intl API, ICU MessageFormat, RTL support, locale detection, hreflang, text expansion, translation workflow, pseudo-localization.
- [14-security.md](14-security.md) — HTTP security headers, CSP with nonces, HSTS, OWASP Top 10, XSS prevention, CSRF, input validation, auth patterns, CORS, dependency auditing.
- [15-discovery-and-communication.md](15-discovery-and-communication.md) — What to ask the user before building, what to default, staged disclosure, red-flag phrases, scope trade-offs, checkpoints, non-frontend concerns, and what never to ask.
- [16-code-style-and-quality.md](16-code-style-and-quality.md) — Cross-language code style, formatter and linter stacks (Prettier, Biome, ESLint flat config, Stylelint, EditorConfig), repository hygiene (.gitignore, .gitattributes, PR template, CODEOWNERS), naming conventions, imports and module organization, comments, error handling, structured logging, dependency management, build toolchain, Conventional Commits and git hooks, and CI enforcement.

## Non-negotiables

Apply these on every project, without exception:

- Every `<img>` has `width`, `height`, and meaningful `alt`.
- The likely LCP image is never lazy-loaded. Use `fetchpriority="high"` on the `<img>` and preload it when the browser would otherwise discover it late.
- Body text is at least 16 px with a line-height of 1.5 and a line length around 60–75 characters.
- Color contrast passes WCAG 2.2 AA (4.5:1 for body text, 3:1 for large text and non-text components). APCA may be used as a secondary design check, not as a substitute for WCAG conformance.
- Focus indicators are visible and have at least 3:1 contrast against the adjacent color.
- `prefers-reduced-motion` is honored; animation is not forced on users who opt out.
- Native HTML elements are used before custom ones: `<button>` before `<div role="button">`, `<dialog>` before a hand-rolled modal.
- Fonts use `font-display: swap` with matched-metric fallbacks or `font-display: optional` for decorative faces.
- No positive `tabindex` values.
- Copy is written in a consistent voice defined up front, not improvised per page.

## Stylistic conventions for generated code

Chapter 16 is the canonical reference for code style, formatters, linters, naming, imports, comments, error handling, logging, dependency management, git workflow, and CI enforcement. Every project inherits that chapter; the handful of conventions below repeat here only because they appear in almost every file:

- Two-space indentation.
- Lowercase HTML element and attribute names.
- Kebab-case file names; class naming per project choice documented in chapter 16.
- Boolean variable names drop the `is` prefix: prefer `visible`, `active`, `disabled`.
- Prefer logical properties (`margin-inline`, `padding-block`) over physical ones.
- Use `rem` for typography and spacing related to text, `px` for hairline borders and small fixed UI details, container or viewport units only where they scale meaningfully with a container.
- Include `.DS_Store` in `.gitignore`.

## How to prompt an LLM to build a website

The LLM reads the guide itself through `AGENTS.md` (the navigation index). Prompts below only name the project and stage; the rules live in the chapters. Keep the prompts short; let the agent pull depth on demand.

### One-prompt kickoff (recommended)

Paste this once at the start of a new project. The agent interviews stage by stage, produces each artifact, and pauses for review.

```
Read web-design-guide/AGENTS.md and drive a new website project per
its "Running order for a new website". At each stage, ask me the
questions you need — one at a time — produce the named artifact,
and pause for my review. Ask only what changes the output. Do not
restate the guide's rules in your answers.
```

### Per-stage prompts

Use these when jumping into a stage mid-project. Each one is a trigger; the agent pulls the relevant chapters from `AGENTS.md`.

- Discovery: `Interview me for brief.md and constraints.md (AGENTS.md stage 1), one question at a time.`
- Voice: `Produce brand.md from brief.md (AGENTS.md stage 2).`
- Architecture: `Produce sitemap.md and wireframes/<page>.md (AGENTS.md stage 3).`
- Copy: `Write copy/<page>.md for each page in sitemap.md (AGENTS.md stage 4). Voice: brand.md.`
- Tokens: `Produce tokens.css (AGENTS.md stage 5) with a WCAG 2.2 AA audit for every foreground/background pair.`
- Stack: `Produce stack.md (AGENTS.md stage 6). List exact packages and current versions.`
- Scaffold: `Scaffold the repository (AGENTS.md stage 7). Commit each sub-step separately.`
- Pages: `Implement /<page> (AGENTS.md stage 8). End with a self-review naming any deviation.`
- Testing: `Wire the testing harness (AGENTS.md stage 9). Output testing.md.`
- Accessibility audit: `Audit /<page> per chapter 05. Report violations with impact; apply fixes after I approve.`
- Performance audit: `Audit /<page> per chapter 06. Same reporting format.`
- Security audit: `Audit the app per chapter 14. Same reporting format.`
- Launch: `Prepare launch.md (AGENTS.md stage 11). Do not deploy until the pre-merge checklist and the security baseline pass.`

### Targeted tasks on an existing project

One-line prompts for common maintenance. The agent finds the matching row in `AGENTS.md` "Task to chapters" and reads what it needs.

- `Revise the pricing-page copy. Voice: brand.md.`
- `Update security for this website.`
- `Improve LCP on /products/*.`
- `Add Dutch and French locales.`
- `Set up tests for the /<path> route.`
- `Add a Combobox component.`
- `Pick a browser support matrix for this project.`
- `Do a pre-merge review on this PR.`

### Common failure modes

- A vague brief poisons every stage. Force specificity in discovery; rewrite if in doubt.
- Constraints left unstated (browser matrix, locales, compliance) surface as rework in the audits. Nail them in discovery.
- Copy written before voice sounds AI-generic. Voice precedes copy.
- Tokens chosen for aesthetics without a contrast audit produce accessible-looking but inaccessible designs.
- Code-quality tooling added at the end rewrites history. Scaffold precedes pages.
- Tests bolted on after the feature set is complete never catch up.
- Accessibility, performance, and security treated as end-of-project gates produce rework. Run them continuously during implementation.

## When in doubt

Favor the simplest thing that works, uses the platform, respects the user, and reads well six months from now.
