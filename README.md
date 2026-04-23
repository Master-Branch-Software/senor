# Web Design Skill

An AI skill that encodes professional-grade web design, frontend engineering, and copywriting practices. Built for LLM-assisted workflows where the default output is junior-level: hard-coded pixels, missing alt text, unlabeled forms, bland copy, and motion that ignores user preferences.

## What this is

- **16 reference chapters** covering philosophy, copywriting, visual design, CSS, accessibility, performance, JavaScript/HTML, stack architecture, anti-patterns, resources, browser compatibility, testing, internationalization, security, discovery, and code style
- **SKILL.md** — navigation index with non-negotiables, task-to-chapter map, and 11-stage running order for new websites
- **Progressive disclosure** — agents load SKILL.md first, then pull only the reference chapters relevant to the current task

## How to use it

### Warp (Oz)

Create a project rule at this directory or a global rule. Paste the "Full load" or "Summary load" prompt from `web-design/references/USAGE.md` as the rule content. Oz will then apply the guide automatically.

### Cursor

Create `.cursor/rules/web-design.mdc` in the project with a pointer to this skill.

### Claude Projects / ChatGPT

Upload `web-design/SKILL.md` plus the numbered chapters as knowledge files.

### Direct prompt

```
Read web-design/SKILL.md and drive a new website project per
its "Running order for a new website". At each stage, ask me the
questions you need — one at a time — produce the named artifact,
and pause for my review. Ask only what changes the output. Do not
restate the guide's rules in your answers.
```

## Structure

```
web-design/
├── SKILL.md              (navigation index + non-negotiables)
├── references/
│   ├── 01-philosophy-and-psychology.md
│   ├── 02-brand-and-copywriting.md
│   ├── 03-visual-design.md
│   ├── 04-css-and-layout.md
│   ├── 05-accessibility.md
│   ├── 06-performance.md
│   ├── 07-javascript-and-html.md
│   ├── 08-stack-and-architecture.md
│   ├── 09-anti-patterns-and-process.md
│   ├── 10-resources.md
│   ├── 11-browser-compatibility.md
│   ├── 12-testing.md
│   ├── 13-internationalization.md
│   ├── 14-security.md
│   ├── 15-discovery-and-communication.md
│   ├── 16-code-style-and-quality.md
│   ├── SUMMARY.md        (condensed cheat sheet)
│   ├── USAGE.md          (integration recipes for editors)
│   └── LINKS.md          (external references)
└── evals/
    └── evals.json        (test prompts)

web-design-workspace/
└── iteration-1/          (eval results with/without skill)

index.html                (landing page for the skill)
```

## Non-negotiables (applied automatically)

- Every `<img>` has `width`, `height`, and meaningful `alt`
- Likely LCP image: `fetchpriority="high"`, never lazy-loaded
- Body text ≥ 16 px, line-height 1.5, line length 60–75 ch
- WCAG 2.2 AA contrast: 4.5:1 body, 3:1 large text/UI
- Visible `:focus-visible` on every focusable element
- `prefers-reduced-motion` honored
- Native HTML first: `<button>`, `<dialog>`, `<details>`, `popover`
- Fonts: `font-display: swap` with matched-metric fallbacks
- No positive `tabindex`
- Two-space indent, kebab-case files, `visible` not `isVisible`, logical properties

## License

Open source. MIT.
