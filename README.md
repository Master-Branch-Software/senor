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

### Direct prompt example

```
Read web-design/SKILL.md and drive a new website project per
its "Running order for a new website". At each stage, ask me the
questions you need — one at a time — produce the named artifact,
and pause for my review. Ask only what changes the output. Do not
restate the guide's rules in your answers.
```
