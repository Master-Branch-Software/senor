# Web Design & Development Master Guide

A reference for building professional-grade websites in LLM-assisted workflows. Load this at the start of any session that will produce or review web code, copy, or visual design.

## Purpose

AI systems default to junior-level output: media-query soup, hard-coded pixels, unlabelled forms, bland copy, low-contrast color, motion that ignores user preferences. This guide encodes the practices that separate professional, maintainable, human-feeling websites from that default.

## How to use

1. Read the relevant chapter first. Color questions → `03-visual-design.md`. Performance questions → `06-performance.md`.
2. Apply stable requirements directly. For new features, verify the support matrix and fallback strategy in `11-browser-compatibility.md` before implementation.
3. Follow the user over the guide when they conflict. Follow the guide over habit when they conflict.

## Chapters

- [SUMMARY.md](SUMMARY.md) — Condensed cheat sheet of every chapter. Load this when context is tight.
- [AGENTS.md](AGENTS.md) — Navigation index for LLMs. Task-to-chapter map, chapter relationships, running order for new projects. Every agent should read this first.
- [USAGE.md](USAGE.md) — How to wire this guide into Warp, Cursor, Claude Projects, Copilot, and other tools; prompt patterns that work.
- [01-philosophy-and-psychology.md](01-philosophy-and-psychology.md) — Laws of UX, Nielsen heuristics, Cialdini principles, Gestalt, emotional design.
- [02-brand-and-copywriting.md](02-brand-and-copywriting.md) — Brand archetypes, voice and tone, copywriting frameworks (AIDA, PAS, BAB, StoryBrand), headlines, hero patterns, microcopy, SEO, JSON-LD.
- [03-visual-design.md](03-visual-design.md) — Hierarchy, spacing, typography, color systems (OKLCH, APCA, Radix).
- [04-css-and-layout.md](04-css-and-layout.md) — Units, responsive strategy, modern CSS (container queries, `:has()`, `@layer`, subgrid, view transitions).
- [05-accessibility.md](05-accessibility.md) — WCAG 2.2, semantic HTML, ARIA rules, keyboard navigation, screen reader testing.
- [06-performance.md](06-performance.md) — Core Web Vitals, bfcache, image optimization, font loading, critical rendering path.
- [07-javascript-and-html.md](07-javascript-and-html.md) — ECMAScript 2025–2026, Temporal, web components, modern HTML primitives.
- [08-stack-and-architecture.md](08-stack-and-architecture.md) — Framework selection, rendering strategies, API styles, Tailwind v4, design tokens.
- [09-anti-patterns-and-process.md](09-anti-patterns-and-process.md) — AI and junior-level mistakes, review checklist, design-and-ship workflow.
- [10-resources.md](10-resources.md) — External references, tools, calculators, libraries.
- [11-browser-compatibility.md](11-browser-compatibility.md) — Support matrix, Baseline, `@supports`, polyfills, cross-browser testing.
- [12-testing.md](12-testing.md) — Testing trophy, Vitest, Playwright, visual regression, accessibility automation.
- [13-internationalization.md](13-internationalization.md) — Intl API, ICU MessageFormat, RTL, hreflang, pseudo-localization.
- [14-security.md](14-security.md) — HTTP headers, CSP, OWASP Top 10, XSS, CSRF, auth patterns.
- [15-discovery-and-communication.md](15-discovery-and-communication.md) — Discovery questions, red-flag phrases, scope trade-offs, staged disclosure.
- [16-code-style-and-quality.md](16-code-style-and-quality.md) — Formatters, linters, Conventional Commits, repository hygiene, CI.

## How to prompt an LLM

The LLM reads `AGENTS.md` (the navigation index) and pulls the relevant chapters. Keep prompts short; let the agent expand on demand.

### One-prompt kickoff

Paste this once at project start. The agent interviews stage by stage, produces each artifact, and pauses for review.

```
Read web-design-guide/AGENTS.md and drive a new website project per
its "Running order for a new website". At each stage, ask me the
questions you need — one at a time — produce the named artifact,
and pause for my review. Ask only what changes the output. Do not
restate the guide's rules in your answers.
```

### Per-stage prompts

Use these when jumping into a stage mid-project.

- Discovery: `Interview me for brief.md and constraints.md (AGENTS.md stage 1), one question at a time.`
- Voice: `Produce brand.md from brief.md (AGENTS.md stage 2).`
- Architecture: `Produce sitemap.md and wireframes/<page>.md (AGENTS.md stage 3).`
- Copy: `Write copy/<page>.md for each page in sitemap.md (AGENTS.md stage 4). Voice: brand.md.`
- Tokens: `Produce tokens.css (AGENTS.md stage 5) with a WCAG 2.2 AA audit for every foreground/background pair.`
- Stack: `Produce stack.md (AGENTS.md stage 6). List exact packages and current versions.`
- Scaffold: `Scaffold the repository (AGENTS.md stage 7). Commit each sub-step separately.`
- Pages: `Implement /<page> (AGENTS.md stage 8). End with a self-review naming any deviation.`
- Testing: `Wire the testing harness (AGENTS.md stage 9). Output testing.md.`
- Accessibility audit: `Audit /<page> per chapter 05. Report violations with impact; apply after approval.`
- Performance audit: `Audit /<page> per chapter 06. Same reporting format.`
- Security audit: `Audit the app per chapter 14. Same reporting format.`
- Launch: `Prepare launch.md (AGENTS.md stage 11). Do not deploy until the pre-merge checklist and security baseline pass.`

### Targeted tasks

One-line prompts for common maintenance. The agent finds the matching row in `AGENTS.md` and reads what it needs.

- `Revise the pricing-page copy. Voice: brand.md.`
- `Update security for this website.`
- `Improve LCP on /products/*.`
- `Add Dutch and French locales.`
- `Set up tests for the /<path> route.`
- `Add a Combobox component.`
- `Pick a browser support matrix for this project.`
- `Do a pre-merge review on this PR.`

### Common failure modes

- **Vague briefs poison every stage.** Force specificity in discovery; rewrite if in doubt.
- **Unstated constraints surface as rework.** Browser matrix, locales, and compliance belong in discovery, not in audit.
- **Copy written before voice reads as generic.** Produce `brand.md` before `copy/<page>.md`.
- **Tokens chosen without contrast audits look accessible but are not.** Audit every foreground/background pair in `tokens.css`.
- **Code-quality tooling added at the end rewrites history.** Scaffold precedes pages.
- **Tests bolted on after the feature set never catch up.** Wire the harness during scaffold.
- **Accessibility, performance, and security as end-of-project gates produce rework.** Run them continuously during implementation.

## When in doubt

Favor the simplest thing that works, uses the platform, respects the user, and reads well six months from now.
