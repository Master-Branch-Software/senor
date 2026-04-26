# The goal of this project

AI is getting good at coding but it's still a unrealiable. AI usually creates a mess from existing codebases and doesn't do good architectural decisions in new ones. AI doesnt follow good practices and doesn't do good architectural decisions. Security is also a concern, not just code.

We're trying to build comprehensive guidelines by analyzing tons of blog posts from real software professionals, official documentations, and professional grade codebases. We're hoping to close this gap and make LLMs more usable, more human. The code we're trying to generate should look human, have good architectural and security decisions. It shouldn't create unnecessary abstractions, over-engineer code, but find balance and create optimal solution.

First visible move from other creators is: https://github.com/forrestchang/andrej-karpathy-skills/blob/main/CLAUDE.md

We're trying to build something better. Something that isn't creating another friction layer but is solving the actual problem. Human interaction is of course still needed if not enough context is provided or something is wrong. It's still important to inform user about decisions. But AI should be more knowledgable. Write better code. Care about security. Not hallucinate and stay real.

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
