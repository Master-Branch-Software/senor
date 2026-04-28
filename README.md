# AI-Readable Engineering Guidelines

**AI-assisted code has recognizable fingerprints.** Purple gradients. Missing alt text. Copy that sounds like a press release. The same nine-section SaaS page, every time. These guidelines fix that — structured, citation-backed references built specifically for how AI agents read and apply context.

> **Early development.** The front-end skill is functional and validated. Ruby, security, and architecture domains are stubs in progress. Expect gaps; contributions are welcome.

<br>

[![Support this project](https://img.shields.io/badge/Support_this_project-%E2%9D%A4-pink?style=for-the-badge&logo=github)](https://github.com/sponsors/YOURUSERNAME)

---

## Contents

```
.
├── SKILL.md              ← AI operating rules (start here)
├── front-end/
│   ├── AGENTS.md         ← front-end skill entry point
│   └── references/       ← 15 chapter files
├── security/             ← in progress
├── ruby/                 ← in progress
├── architecture/         ← in progress
├── scripts/              ← utility scripts
├── CONTRIBUTING.md       ← how to add a domain or chapter
└── LICENSE
```

**Jump to:** [The problem](#the-problem) · [What this is](#what-this-is) · [Works with](#works-with) · [How to use](#how-to-use) · [How it works](#how-it-works) · [Scripts](#scripts) · [Status](#status) · [Contributing](#contributing) · [License](#license)

---

## The problem

AI coding tools produce the same mistakes every session. Hard-coded pixel values. Unlabeled form fields. Body copy built from nominalizations and hedging verbs. Hero sections with a purple gradient and two competing CTAs. The output looks correct on screen and breaks under inspection — accessibility tree empty, contrast failing, copy that could describe any competitor in the category.

The root cause isn't capability. It's context. Without structured references, an agent defaults to the median of its training data: Tailwind component galleries, SaaS landing-page tutorials, Dribbble shots. That median is what the "AI look" is made of.

More instructions in a system prompt don't fix it. A rules dump is not a reference. An agent needs the same thing a junior engineer needs: a structured body of knowledge it can navigate by task, not a wall of text to skim once.

---

## What this is

A library of AI-readable skill files — structured guidelines organized by domain, written for how language models read and apply context.

Each domain has three layers:

- **Entry point** (`AGENTS.md`) — tells the agent which chapter to load for the current task. Minimal. No chapter dumps.
- **Chapter files** (`NN-topic.md`) — the actual guidelines. Terse, imperative, cited. Written to change agent behavior, not to read well in isolation.
- **Source files** (`NN-topic.sources.md`) — citations backing every claim. Human-authored only: specs, MDN, engineering blogs, production codebases.

The front-end domain ships 15 chapters: philosophy and psychology, brand and copywriting, visual design, CSS and layout, accessibility, performance, JavaScript and HTML, stack and architecture, anti-patterns, browser compatibility, testing, internationalization, security, discovery and communication, and code style. It includes a 14-pattern AI-copy checklist that cuts the structural fingerprints that make AI-generated prose recognizable.

---

## Works with

These guidelines are plain Markdown files. Any AI agent that can read files can use them.

| Tool | Integration method |
| --- | --- |
| **Claude Code** | Clone into `.claude/skills/` — auto-discovered from SKILL.md frontmatter |
| **Windsurf** | Clone into your skills directory — same SKILL.md convention |
| **Cursor** | Drop `front-end/AGENTS.md` into `.cursor/rules/` as a `.mdc` file |
| **GitHub Copilot** | Copy `front-end/references/SUMMARY.md` into `.github/copilot-instructions.md` |
| **Cline** | Reference the repo path in your project rules |
| **Any AI with file access** | Point the agent at `SKILL.md` or the relevant `AGENTS.md` in a prompt |

---

## How to use

### Claude Code and Windsurf

Clone the repo into your project's skill directory. Claude Code discovers skills automatically based on the SKILL.md frontmatter description.

```bash
# Add as a git submodule (recommended — keeps it updatable)
git submodule add https://github.com/Master-Branch-Software/ai-web-designer .claude/skills/ai-web-designer

# Or clone directly
git clone https://github.com/Master-Branch-Software/ai-web-designer .claude/skills/ai-web-designer
```

The agent will load `SKILL.md` for cross-domain rules, then `front-end/AGENTS.md` when the task involves anything rendered in a browser.

### Cursor

Copy `front-end/AGENTS.md` into your project as `.cursor/rules/front-end.mdc`. For the full reference, copy individual chapter files from `front-end/references/` into additional rule files.

### GitHub Copilot

Copilot reads `.github/copilot-instructions.md` as a single file. Copy the content of `front-end/references/SUMMARY.md` there for a condensed reference that fits within Copilot's context budget.

### Any other AI with file access

Reference the skill in your system prompt or first message:

```
Read SKILL.md, then front-end/AGENTS.md. Use the task-to-chapter map
to find the relevant chapter for this task and read it before responding.
```

For a new website project specifically:

```
Read front-end/AGENTS.md and follow its "Running order for a new website".
At each stage, produce the named artifact and pause for my review.
Ask only what changes the output. Do not restate the guide's rules.
```

---

## How it works

The skill system uses progressive disclosure. An agent reads the entry point first — a short file with a task-to-chapter map — and pulls only the chapters the current task actually needs. A bug fix loads one chapter. A new website from scratch loads all of them, in a defined order.

Each chapter is written for LLM consumption: terse, imperative, no padding. Rules are specific enough to change output ("use `overflow-x: auto` on `<pre>` elements") rather than general enough to ignore ("write clean code"). Every rule has a citation in a matching sources file, so there's an audit trail for every claim.

The root `SKILL.md` holds operating principles that apply across all domains: read before building, artifacts before code, run domain checklists before submitting. Domain `AGENTS.md` files handle routing and domain-specific gates (the front-end skill runs a 14-item AI-copy scan and an AI-fingerprint audit before any artifact is submitted).

For the full structural breakdown — file conventions, how to add a chapter, how to run evals — see [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Scripts

The `scripts/` directory contains utilities for working in this repository.

| Script | What it does |
| --- | --- |
| `scripts/new-domain <name>` | Scaffolds a new domain directory with a filled AGENTS.md template |
| `scripts/check-sources [domain]` | Verifies every chapter file has a matching `.sources.md` citation file |
| `scripts/format [--check]` | Runs Prettier across all non-ignored files; `--check` mode for CI |

Scripts require Bash and — for `format` — Node.js with `npx` available.

---

## Status

| Domain | Chapters | Evals | Notes |
| --- | --- | --- | --- |
| `front-end/` | 15 of 15 | 2 iterations | Functional and validated |
| `security/` | 0 | 0 | Stub — entry point only |
| `ruby/` | 0 | 0 | Stub — planned next |
| `architecture/` | 0 | 0 | Stub — planned |

Planned work: Ruby chapter files, Rails subdomain, architecture decision records pattern, eval harness improvements, and a validate-citations CI check.

---

## Contributing

Contributions that improve the chapter files, add citations, or build out new domains are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request — it covers the file structure, voice rules, citation requirements, and the eval convention for validating that a rule change actually changes agent behavior.

The short version: every new rule needs a human-authored source. AI-generated content does not qualify as a citation.

---

## License

Use these guidelines to build your own work — portfolios, client sites, apps, internal tools, anything else — commercial or not. Load them into AI agents on a per-session or per-project basis, drop them into Claude Code or Cursor, reference them in a system prompt while you build. The websites, apps, and software you produce are entirely yours.

You **may not** ship the guidelines as part of a commercial AI product. Specifically: no repackaging or selling them as a standalone product, no bundling into a paid SaaS feature or AI service, and no embedding them as a permanent component of a distributed AI tool, language model, or AI-powered product — including as training data, fine-tuning data, or a system prompt that ships with your product.

If you run a commercial AI tool or product and want to incorporate these guidelines, get in touch and we'll work out an agreement — we're easy to work with. bert@masterbranchsoftware.com or via [masterbranchsoftware.com](https://masterbranchsoftware.com/contact).

A note on the integration recipes above: pasting chapter files into your own project (e.g., `.cursor/rules/`, `.github/copilot-instructions.md`) is fine for internal use within your organization. If you publish or redistribute those copies — say, in a public template repo — Section 7 of the LICENSE asks you to include the LICENSE terms with the copies and flag any modifications.

This project is licensed under the [Master Branch Source License v2.1](LICENSE). The LICENSE file is authoritative; this section is a plain-language summary for orientation.

This license was inspired by [CorridorKey](https://github.com/nikopueringer/CorridorKey) by Corridor Digital — go check out their work and show them some support!

---

## Support this project

If these guidelines save you time, consider supporting continued development.

[![Support on Ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/YOURUSERNAME)
