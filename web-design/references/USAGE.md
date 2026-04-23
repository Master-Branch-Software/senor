# How to Use This Guide in Prompts

The guide is designed to prime any LLM (Warp/Oz, Cursor, Claude, ChatGPT, Gemini, Copilot, local models) with the knowledge needed to produce professional, maintainable websites. This document describes the invocation patterns that work in practice.

## Start with `SKILL.md`

`SKILL.md` is the navigation index. An agent reads it first, then pulls the chapters relevant to the task from the task-to-chapter map. For most work, priming with `SKILL.md` is enough — the agent will request deeper chapters on demand.

## Four ways to load the guide

Pick one based on how much context the session has available.

### 1. Full load — best quality, largest context

Load `SKILL.md` plus every numbered chapter from `references/` at the start of a session. Roughly 80–100 K tokens. Use when:

- Starting a new project or significant refactor.
- Producing multi-page designs or a design system.
- The model is the primary author and will make many decisions.

Prompt opener:

```
Before you answer anything, read these files and treat them as
canonical conventions for the rest of this session:

- web-design/SKILL.md
- web-design/references/01-philosophy-and-psychology.md
- web-design/references/02-brand-and-copywriting.md
- web-design/references/03-visual-design.md
- web-design/references/04-css-and-layout.md
- web-design/references/05-accessibility.md
- web-design/references/06-performance.md
- web-design/references/07-javascript-and-html.md
- web-design/references/08-stack-and-architecture.md
- web-design/references/09-anti-patterns-and-process.md
- web-design/references/10-resources.md
- web-design/references/11-browser-compatibility.md
- web-design/references/12-testing.md
- web-design/references/13-internationalization.md
- web-design/references/14-security.md
- web-design/references/15-discovery-and-communication.md
- web-design/references/16-code-style-and-quality.md

Apply every non-negotiable from SKILL.md without exception.
When a rule conflicts with a habit, follow the rule. When a rule
conflicts with my explicit instruction, follow my instruction.
```

### 2. Agent-index load — recommended default

Load `SKILL.md` and `SKILL.md`. Roughly 5–8 K tokens. Use when:

- Most day-to-day work, including new-project kickoffs.
- You want the agent to pull deeper chapters only as needed.
- The environment supports on-demand file reads (Warp/Oz, Cursor, Claude Code, Codex).

Prompt opener:

```
Read web-design/SKILL.md.
Apply the non-negotiables from SKILL.md on every suggestion. Use
the "Task-to-chapter map" in SKILL.md
to read next for the current task. Ask only what changes the
output.
```

### 3. Summary load — lightweight priming

Load `SUMMARY.md` alone. Roughly 3 K tokens. Use when:

- Tight context budgets (short models, long sessions, many files already loaded).
- Quick Q&A or small incremental changes.
- Sanity-checking existing work against the non-negotiables.

Prompt opener:

```
Read web-design/references/SUMMARY.md and apply its non-negotiables
to every suggestion. If depth is needed on a topic, ask me for
the corresponding numbered chapter.
```

### 4. Targeted load — per-task

Load `SKILL.md` (short) plus the one or two chapters relevant to the current task. Use when working on a single concern for a block of time.

Task-to-chapter mapping:

- Visual design, typography, color, layout visuals, dark mode — `03-visual-design.md`.
- CSS authoring, responsive behavior, animation, GSAP — `04-css-and-layout.md`.
- Accessibility audits, keyboard flows, screen reader support — `05-accessibility.md`.
- Core Web Vitals, image/font optimization, third-party scripts — `06-performance.md`.
- Modern JavaScript features, HTML elements, web components, runtimes — `07-javascript-and-html.md`.
- Framework choice, Tailwind v4, shadcn/ui, tokens, state, forms — `08-stack-and-architecture.md`.
- Copywriting, hero sections, brand voice, SEO, structured data — `02-brand-and-copywriting.md`.
- Design reviews, anti-pattern audits, pre-merge checks, AI pitfalls — `09-anti-patterns-and-process.md`.
- Research or tool lookup — `10-resources.md`.
- Browser support matrix, `@supports`/feature detection, polyfill choices, cross-browser testing — `11-browser-compatibility.md`.
- Testing strategy, Vitest, Playwright, MSW, visual regression, CI — `12-testing.md`.
- Internationalization, Intl API, RTL, hreflang, locale patterns — `13-internationalization.md`.
- Security headers, CSP, XSS, CSRF, auth, CORS, OWASP — `14-security.md`.
- Discovery questions for a new brief, what to default, when to ask, checkpoints, and communication patterns with the user — `15-discovery-and-communication.md`.
- Code style and formatting, ESLint/Prettier/Biome/Stylelint setup, `tsconfig` baseline, repository hygiene, naming, imports, error handling, structured logging, dependency management, Conventional Commits, git hooks, CI enforcement — `16-code-style-and-quality.md`.

## Integrations

### Warp (Oz) rules

1. Open Warp settings → Rules.
2. Create a project rule at `/home/bert/Documents/dev/ai-web-designer` or a global rule.
3. Paste the "Full load" or "Summary load" prompt opener above as the rule content.
4. Oz will then apply the guide automatically to conversations in that directory.

### Cursor rules (`.cursor/rules`)

Create `.cursor/rules/web-design.mdc` in the project:

```
---
description: Design, code, copy, and accessibility conventions for websites
globs: ["**/*.{html,css,js,ts,jsx,tsx,mdx,astro,svelte,vue}"]
alwaysApply: true
---

Follow the conventions in /home/bert/Documents/dev/ai-web-designer.
The non-negotiables in SKILL.md apply to every suggestion.
Load SUMMARY.md for context; ask to load numbered chapters on demand.
```

### Claude Projects / ChatGPT custom instructions

Paste `SUMMARY.md` into the project's knowledge base or custom-instructions field. Add one sentence: "These are the canonical conventions for web design and code in this project."

For Claude Projects specifically, upload every numbered chapter as a knowledge file so they can be retrieved on demand.

### GitHub Copilot (workspace-level)

Create `.github/copilot-instructions.md` at the repo root with a short pointer:

```
Use the conventions in /home/bert/Documents/dev/ai-web-designer for all HTML, CSS,
JavaScript, and copy. Non-negotiables are listed in SKILL.md.
```

### IDE-less / CLI agents

Pipe the summary in as context:

```
<prime>
<content of SUMMARY.md>
</prime>

<task>
<your request>
</task>
```

## Prompt patterns that work well

Each pattern below assumes the guide has been loaded using one of the methods above.

### Starting a new page

```
Design a pricing page for <brand>. Brand voice is documented in
brand.md. Apply the hero pattern and StoryBrand structure from
02-brand-and-copywriting.md. Output copy first, wireframe second,
component breakdown third. Do not write CSS yet.
```

### Auditing an existing page

```
Audit /index.html against the pre-merge checklist in
09-anti-patterns-and-process.md. For each item that fails, quote
the relevant non-negotiable from SKILL.md and propose a fix.
Do not apply edits until I approve.
```

### Adding a component

```
Create a <Combobox> component. Follow composition-over-configuration
rules from 08-stack-and-architecture.md. Use Radix or React Aria for
a11y primitives — do not roll the a11y behavior from scratch.
Include light and dark themes using OKLCH tokens.
```

### Performance regression hunt

```
The page at /products/* has an LCP of 3.2 s in CrUX. Using
06-performance.md, list candidate causes ordered by likely impact
and suggest one concrete fix for each. Verify each suggestion against
the LCP phases.
```

### Copy revision

```
Rewrite the hero copy of /pricing. Voice attributes:
plainspoken-but-not-dumbed-down, genuine-but-not-overly-sincere.
Target feel: confident. Follow the microcopy rules and the hero
section pattern from 02-brand-and-copywriting.md.
```

### Accessibility fix

```
This modal traps focus but does not return focus to the trigger on
close. Fix per 05-accessibility.md, using the dialog/showModal() path
if possible, otherwise implement the documented focus-trap pattern.
```

### Stack selection

```
I need a static marketing site with a blog and one interactive
pricing calculator. Walk through 08-stack-and-architecture.md and
recommend the framework. Justify the choice against the criteria
listed there.
```

## Priority of rules

If instructions conflict, resolve in this order:

1. The user's explicit message in the current turn.
2. Project-specific rules (for example `WARP.md`, `.cursor/rules`) that override the guide.
3. The non-negotiables in `SKILL.md`.
4. The specific chapter relevant to the task.
5. `SUMMARY.md` when a chapter is not available.
6. The model's general training.

## Keeping the guide current

Treat the guide as a living document.

- Revisit quarterly. Check WCAG, ECMAScript, CSS, framework, and browser changelogs for any new primitives worth promoting into the non-negotiables.
- When a pattern keeps getting re-asked (shadow DOM styling, motion timing, CLS debugging), capture the refined answer in the relevant chapter.
- Remove resources that go stale.
- Keep the tone consistent: formal, technical, no "we/our", no emojis, no subjective commentary.

## Quality signals

When the guide is working, expect to see:

- `<img>` tags that always include `width`, `height`, `alt`, and an appropriate `loading` attribute.
- Likely LCP images prioritized with `fetchpriority="high"` and preloaded when the browser would otherwise discover them late.
- CSS authored in `@layer`-scoped files with logical properties and modular tokens.
- Copy that names the audience and leads with a verb.
- Components built on Radix or React Aria rather than hand-rolled.
- Motion that honors `prefers-reduced-motion` by default.
- Prettier, ESLint flat config, Stylelint, EditorConfig, `commitlint`, and `lint-staged` wired up on every fresh repo.
- Bundle, a11y, typecheck, test, and Lighthouse checks wired into CI with explicit budgets.

When the guide is not working, outputs fall back to junior defaults: `<div onClick>`, pixel everything, generic stock imagery, "Learn more" buttons, and no respect for `prefers-reduced-motion`. That is the signal to re-prime the session.

## A minimal session primer

For single-turn help in a fresh session where loading the guide is not possible, paste this block:

```
Follow these non-negotiables:
- <img> always has width, height, alt; likely LCP image is never lazy-loaded, uses fetchpriority="high", and is preloaded only when discovered late.
- Body text ≥ 16 px, line-height 1.5, line length 60–75 ch.
- WCAG 2.2 AA contrast; visible :focus-visible; prefers-reduced-motion respected.
- Native HTML first (<button>, <dialog>, <details>, popover attribute).
- OKLCH color, fluid type via clamp(), container queries for component breakpoints.
- Tailwind v4 + shadcn/ui + Radix as the default React stack; Astro for content sites.
- No positive tabindex; no color-only meaning; no 100vh on mobile (use 100dvh).
- Copy uses verbs on CTAs, labels on inputs, concrete error messages.
- Drop "is" prefix from boolean names; prefer logical properties; two-space indent.
- Prettier + ESLint flat config + Stylelint + EditorConfig on every project; Conventional Commits; no console.log in production; strict tsconfig.
Produce work to these standards without asking for confirmation.
```

That block alone raises baseline output meaningfully, even without the rest of the guide.
