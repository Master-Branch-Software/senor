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

- `SUMMARY.md` — Condensed cheat sheet of every chapter. Load this when context is tight.
- `USAGE.md` — How to wire this guide into Warp, Cursor, Claude Projects, Copilot, and other tools; prompt patterns that work.
- `01-philosophy-and-psychology.md` — Intent, craft, Laws of UX, Nielsen heuristics, Cialdini's 7 principles of persuasion, Gestalt principles, emotional design.
- `02-brand-and-copywriting.md` — Jungian brand archetypes, voice and tone systems, copywriting frameworks (AIDA, PAS, BAB, StoryBrand), microcopy, hero section patterns.
- `03-visual-design.md` — Visual hierarchy, spacing scales, typography (scales, variable fonts, fluid), color systems (OKLCH, APCA, Radix, tooling), design styles.
- `04-css-and-layout.md` — CSS units, responsive strategy, modern CSS (container queries, `:has()`, `@layer`, subgrid, view transitions, `@property`, `@function`, `if()`), motion principles.
- `05-accessibility.md` — WCAG 2.2, semantic HTML, the five ARIA rules, keyboard navigation, focus management, screen reader testing, automated auditing.
- `06-performance.md` — Core Web Vitals (LCP, INP, CLS), bfcache, image optimization, font loading, third-party script strategy, critical rendering path, RUM.
- `07-javascript-and-html.md` — ECMAScript 2025 and 2026, Temporal, iterator helpers, the Signals proposal, web components, runtime selection (Node/Deno/Bun), modern HTML primitives.
- `08-stack-and-architecture.md` — Framework selection criteria, Tailwind v4, shadcn/ui, design tokens, component architecture, co-location, state management.
- `09-anti-patterns-and-process.md` — Common AI and junior-level mistakes to avoid, review checklist, design-and-ship workflow.
- `10-resources.md` — Trusted external references, tools, calculators, and libraries.
- `11-browser-compatibility.md` — Choosing a support matrix, Baseline, `@supports` and feature detection, polyfills, per-feature status, cross-browser testing.
- `12-testing.md` — The testing trophy, Vitest + Testing Library, MSW, Playwright E2E, visual regression, accessibility automation, CI integration.
- `13-internationalization.md` — Intl API, ICU MessageFormat, RTL support, locale detection, hreflang, text expansion, translation workflow, pseudo-localization.
- `14-security.md` — HTTP security headers, CSP with nonces, HSTS, OWASP Top 10, XSS prevention, CSRF, input validation, auth patterns, CORS, dependency auditing.

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

- Two-space indentation.
- Lowercase HTML element and attribute names.
- Kebab-case for file names and CSS class names.
- Boolean variable names drop the `is` prefix: prefer `visible`, `active`, `disabled` over `isVisible`, `isActive`, `isDisabled`.
- Prefer logical properties (`margin-inline`, `padding-block`) over physical ones.
- Use `rem` for typography and spacing related to text, `px` for hairline borders and small fixed UI details, container or viewport units only where they scale meaningfully with a container.
- Include `.DS_Store` in `.gitignore`.

## How to prompt an LLM to build a website

A twelve-stage workflow. Each stage has an objective, a prompt template, and the artifact it should produce. The stages build on each other; skipping one degrades every stage that follows. Start every session by pasting the "Full load" opener from `USAGE.md`, or at minimum the "minimal session primer" block at the end of that file.

### Rules that apply to every stage

- Produce the artifact in writing before any code. Text reveals unclear thinking; code hides it.
- Keep answers specific. Reject vague verbs ("empower," "unleash," "transform," "learn more"), generic audiences ("users," "customers"), and stock promises ("the leading," "the #1").
- End each stage with a short self-review that names any deviation from the guide.
- Treat every artifact as read-only input for the next stage. If a later stage exposes a flaw upstream, fix the upstream artifact before continuing.

### Stage 1 — Brief

Objective: a one-page document that answers who, what, why, and feel.

Prompt:

```
Act as a senior product designer. Interview me to produce a project
brief. Ask one question at a time. Capture:
- audience (one sentence, job-to-be-done, not demographic)
- primary action (one sentence, one verb)
- differentiator (one sentence, grounded in truth)
- target feel (one adjective)
- success metric
- known constraints (budget, deadline, tech, compliance)
When finished, summarize the brief in under 150 words and save it
as brief.md.
```

### Stage 2 — Brand voice

Objective: archetype, personality, voice attributes, and tone map.

Prompt:

```
From brief.md, pick one primary Jungian archetype from
02-brand-and-copywriting.md and justify it in two sentences. Then
write brand.md containing:
- 3-5 voice attributes, each with a "but not" modifier
- a tone map for onboarding, error, destructive confirmation,
  celebration, and marketing copy
- a "words to use / words to avoid" pair of lists
```

### Stage 3 — Information architecture

Objective: sitemap and page goals.

Prompt:

```
Using brief.md, propose a sitemap with 3-7 top-level pages. For each
page, list:
- the single primary goal
- the primary CTA (verb + noun)
- the evidence types that belong on it (testimonial, screenshot,
  stat, logo bar)
Output as a markdown outline in sitemap.md.
```

### Stage 4 — Copywriting

Objective: hero and section copy per page.

Prompt:

```
Write the hero section for each page in sitemap.md, following the
hero pattern in 02-brand-and-copywriting.md:
- a 6-12-word headline that names audience and benefit
- a 15-25-word subhead with concrete proof
- a verb+noun primary CTA
- an optional lower-commitment secondary CTA
- one evidence element
Voice: brand.md. Do not invent customer names or statistics.
Output under copy/<page>.md.
```

### Stage 5 — Design tokens

Objective: colors, type, spacing, radius, shadow, with accessibility audit.

Prompt:

```
Propose design tokens for the site. Generate:
- a 12-step OKLCH scale for 1 neutral and 1 accent, in both light
  and dark modes, following the Radix model
- a type scale with 8 steps built from a 1.2 ratio, expressed as
  clamp() values
- an 8-pt spacing scale with 9 steps
- a radius scale (sm, md, lg, xl, 2xl) and a shadow scale
  (sm, md, lg, xl)
Audit every foreground/background pair against WCAG 2.2 AA and
report the contrast ratios. Output tokens.css with @theme or :root
custom properties.
```

### Stage 6 — Wireframes

Objective: low-fi structural plan per page.

Prompt:

```
For each page in sitemap.md, produce a wireframe as a markdown
outline showing sections top-to-bottom. For each section, state:
- its role (hero, feature grid, testimonial, pricing, faq, cta)
- the hierarchy (h1, h2, key body)
- the interactive elements
- approximate vertical density (sparse, regular, dense)
Do not describe visuals or colors. Output wireframes/<page>.md.
```

### Stage 7 — Stack

Objective: framework and library selection, justified.

Prompt:

```
Using 08-stack-and-architecture.md, pick a framework, styling
approach, component primitives, image pipeline, and deployment
target for this project. Justify each choice in one sentence
against the criteria in that chapter. List exact packages and
current versions. Output stack.md.
```

### Stage 8 — Scaffold

Objective: a minimal running project that satisfies the non-negotiables before any page is built.

Prompt:

```
Scaffold the project chosen in stack.md with:
- the folder structure from 08-stack-and-architecture.md (by-feature
  or by-type; pick one and justify)
- tokens.css wired into the design system
- a modern CSS reset inside @layer reset, base, components,
  utilities
- primitive components: Button, Card, Section, Container, Stack,
  Grid, built on the tokens
- accessibility defaults: visible :focus-visible, skip link,
  prefers-reduced-motion handler, viewport meta without
  user-scalable=no
- lint, format, type-check, and test runners configured
- CI workflow running type check, tests, axe, Lighthouse CI, and
  bundle-size budgets
Commit each step separately.
```

### Stage 9 — Pages

Objective: implement each page from its wireframe and copy using the primitives.

Prompt:

```
Implement /<page> from wireframes/<page>.md and copy/<page>.md
using the primitives from Stage 8. Requirements:
- every <img> has width, height, alt, and appropriate loading and
  fetchpriority attributes
- type sizes come from the token scale, not hardcoded values
- layouts use grid auto-fit or container queries where appropriate
- focus is visible on every interactive element
- no positive tabindex, no <div onClick>, no <a href="#">
End with a self-review that names any deviation from the guide.
```

### Stage 10 — Accessibility pass

Objective: zero automated violations, clean keyboard and screen-reader flow.

Prompt:

```
Audit /<page> per 05-accessibility.md. Report:
- axe and Lighthouse violations with counts and severities
- a keyboard walk-through covering tab order, focus return on
  modal close, and skip link
- a screen-reader walk-through (VoiceOver or NVDA) noting missing
  names, landmarks, or live regions
Propose fixes ordered by impact. Apply them only after I approve
each.
```

### Stage 11 — Performance pass

Objective: LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1 in field data.

Prompt:

```
Audit /<page> per 06-performance.md. Break LCP into its four
phases; identify INP offenders, especially third-party scripts;
verify every image reserves dimensions and that the LCP image is
preloaded with fetchpriority="high"; confirm fonts use metric-
matched fallbacks or font-display: optional.
Propose fixes ordered by expected impact and the CI budget changes
needed to prevent regressions.
```

### Stage 12 — Launch

Objective: staged rollout with monitoring and a rollback plan.

Prompt:

```
Prepare a launch plan:
- a staging deploy with a preview URL
- smoke tests covering the primary user flow defined in brief.md
- monitoring hooks: web-vitals RUM, error tracking with source maps,
  uptime checks
- a rollback procedure
- a 30-day and 90-day review tied to the success metric in brief.md
Do not deploy until the pre-merge checklist in
09-anti-patterns-and-process.md passes for every page.
```

### Common failure modes

- A vague brief poisons every stage that follows. Force specificity in Stage 1; rewrite if in doubt.
- Copy written before voice is decided sounds AI-generic. Stage 2 always precedes Stage 4.
- Tokens chosen for aesthetics without contrast audit produce accessible-looking but inaccessible designs. Stage 5 requires the audit.
- Pages written before primitives exist re-invent the same Button five times. Stage 8 always precedes Stage 9.
- Accessibility and performance treated as end-of-project gates produce rework. Stages 10 and 11 are run continuously during Stage 9, not after it.

## When in doubt

Favor the simplest thing that works, uses the platform, respects the user, and reads well six months from now.
