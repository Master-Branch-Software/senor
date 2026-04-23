# Web Design & Development Master Guide

A curated, opinionated reference for designing and building professional-grade websites. Written to be loaded at the start of any LLM session that will produce or review web code, copy, or visual design decisions.

## Purpose

The default output of AI systems tends toward junior-level code and generic design: media-query soup, hard-coded pixel values, unlabelled form fields, bland copy, low-contrast color, and motion that ignores user preferences. This guide encodes the practices that separate professional, maintainable, human-feeling websites from that default.

## How to use

1. Read the relevant chapter before touching code. If a task concerns color, read `03-visual-design.md`. If it concerns performance, read `06-performance.md`.
2. Apply the principles literally. When the guide says "use `fetchpriority=\"high\"` on the LCP image," do it on every LCP image.
3. Prefer the documented tool or pattern over inventing a new one. Every recommendation here beats the average ad-hoc solution.
4. When a rule conflicts with a user requirement, follow the user. When a rule conflicts with a habit, follow the rule.

## Chapters

- `01-philosophy-and-psychology.md` — Intent, craft, Laws of UX, Nielsen heuristics, Cialdini's 7 principles of persuasion, Gestalt principles, emotional design.
- `02-brand-and-copywriting.md` — Jungian brand archetypes, voice and tone systems, copywriting frameworks (AIDA, PAS, BAB, StoryBrand), microcopy, hero section patterns.
- `03-visual-design.md` — Visual hierarchy, spacing scales, typography (scales, variable fonts, fluid), color systems (OKLCH, APCA, Radix, tooling), design styles.
- `04-css-and-layout.md` — CSS units, responsive strategy, modern CSS (container queries, `:has()`, `@layer`, subgrid, view transitions, `@property`, `@function`, `if()`), motion principles.
- `05-accessibility.md` — WCAG 2.2, semantic HTML, the five ARIA rules, keyboard navigation, focus management, screen reader testing, automated auditing.
- `06-performance.md` — Core Web Vitals (LCP, INP, CLS), image optimization, font loading, third-party script strategy, critical rendering path, RUM.
- `07-javascript-and-html.md` — ECMAScript 2025 and 2026, Temporal, iterator helpers, the Signals proposal, web components, runtime selection (Node/Deno/Bun), modern HTML primitives.
- `08-stack-and-architecture.md` — Framework selection criteria, Tailwind v4, shadcn/ui, design tokens, component architecture, co-location, state management.
- `09-anti-patterns-and-process.md` — Common AI and junior-level mistakes to avoid, review checklist, design-and-ship workflow.
- `10-resources.md` — Trusted external references, tools, calculators, and libraries.

## Non-negotiables

Apply these on every project, without exception:

- Every `<img>` has `width`, `height`, and meaningful `alt`.
- The LCP image is preloaded with `fetchpriority="high"` and never lazy-loaded.
- Body text is at least 16 px with a line-height of 1.5 and a line length around 60–75 characters.
- Color contrast passes WCAG 2.2 AA (4.5:1 for body text, 3:1 for large text and non-text components) or the equivalent APCA threshold.
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

## When in doubt

Favor the simplest thing that works, uses the platform, respects the user, and reads well six months from now.
