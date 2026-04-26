# Web Design Skill

Point your AI agent at `web-design/SKILL.md`. It stops producing hard-coded pixels, missing alt text, unlabeled forms, broken contrast, and copy that sounds like a press release.

Built for LLM-assisted workflows where the default output fails in the same ways every time. This skill encodes 15 reference chapters of professional-grade frontend engineering, visual design, and copywriting practice, including a 14-pattern AI-DNA copy checklist to cut the structural fingerprints that make AI-generated prose sound generic.

## What this is

- **15 reference chapters** covering philosophy, brand and copywriting (including AI-DNA pattern audit), visual design, CSS, accessibility, performance, JavaScript/HTML, stack architecture, anti-patterns, browser compatibility, testing, internationalization, security, discovery, and code style
- **SKILL.md** — navigation index with non-negotiables, task-to-chapter map, and 11-stage running order for new websites
- **Progressive disclosure** — agents load SKILL.md first, then pull only the chapters the current task needs. No 15-chapter dump.

## How to use it

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
│   ├── 10-browser-compatibility.md
│   ├── 11-testing.md
│   ├── 12-internationalization.md
│   ├── 13-security.md
│   ├── 14-discovery-and-communication.md
│   ├── 15-code-style-and-quality.md
│   ├── SUMMARY.md        (condensed cheat sheet)
│   └── USAGE.md          (integration recipes for editors)
└── evals/
    └── evals.json        (test prompts)

web-design-workspace/
├── iteration-1/          (eval results with/without skill)
└── iteration-2/          (6-eval run with summary)

index.html                (landing page for the skill)
```

