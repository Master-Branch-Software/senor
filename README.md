<p align="center">
  <img src="logo.webp" alt="SeñorDevBot logo" width="300" height="300">
</p>

**AI-assisted code is unreliable.** It mishandles existing codebases, ducks the architectural decisions that matter, ignores security, over-engineers easy problems, and hallucinates the rest. These guidelines give an AI agent a structured reference drawn from real engineering practice, so the output looks human, holds up under review, and reflects your project rather than the average of its training data.

SeñorDevBot is a library of plain-Markdown guidelines, organized by domain and split into chapters the agent loads only when the task calls for them. Every rule is backed by a citation to a human-authored source like an official spec, MDN, an established engineering blog, a book, or a production codebase. AI-generated material does not qualify.

The deeper rationale (progressive disclosure, citation requirements, how rules are validated) lives in [CONTRIBUTING.md](CONTRIBUTING.md).

**This is still early in development.** Contributions are very welcome!

---

## Table of contents

- [Layout](#layout)
- [How to use](#how-to-use)
- [Prompt frameworks](#prompt-frameworks)
- [Tooling](#tooling)
- [Contributing](#contributing)
- [License](#license)
- [Support](#support)

---

## Layout

```
.
├── SKILL.md                          ← AI operating rules — start here
├── front-end/                        ← a domain (front-end, security, ruby, etc.)
│   ├── AGENTS.md                     ← entry point — tells the agent which chapter to load
│   ├── topic.md                      ← chapter — terse, imperative guidelines
│   └── topic.sources.md              ← citations backing every claim
├── install, update                   ← installer + updater for non-Claude agents
├── templates/                        ← rule-file templates the installer reads
├── AGENTS.md, .cursor/, .windsurf/,  ← rules applied when working on senor itself
│   .clinerules/, .github/             (kept in sync with templates manually)
├── scripts/                          ← utilities for working with AI context
├── CONTRIBUTING.md                   ← how to add a domain, chapter, or citation
└── LICENSE
```

---

## How to use

The agent reads `SKILL.md` for cross-domain rules, then loads the relevant `AGENTS.md` when the task matches a domain.

### Quick install

Clone the repo to a folder named `senor` anywhere you like (your home dir, `~/Documents`, a projects folder), then run the installer:

```bash
git clone https://github.com/Master-Branch-Software/senor
cd senor
./install
```

You'll get an interactive menu to pick which agents to set up (Cursor, Windsurf, Cline, Codex CLI, GitHub Copilot), the install scope (this project, user-global, or a custom path), and whether to enable an auto-update task that pulls the latest content and reinstalls. Frequency defaults to weekly (Sundays 3am local time) and can be set to daily with `--update-frequency daily` or via the prompt. The task runs through cron on macOS/Linux and Task Scheduler on Windows (from Git Bash).

The user-global scope writes to each agent's documented user-level discovery path: `~/.codex/AGENTS.md` for Codex, `~/.codeium/windsurf/memories/global_rules.md` for Windsurf, `~/Documents/Cline/Rules/senor.md` for Cline. Cursor and Copilot have no documented user-global file path and will be skipped under the global scope; install them per-project instead.

For Claude Code, clone the repo into `.claude/skills/senor/` (project) or `~/.claude/skills/senor/` (user) — it auto-loads `SKILL.md` from there. The installer doesn't manage Claude Code; the clone is enough.

### Via your AI agent

Don't have git or want help? Paste this into Claude Code, ChatGPT, Cursor's chat, or any other agent that can run shell commands:

> Clone `github.com/Master-Branch-Software/ai-web-designer` into a folder named `senor` (in my home directory by default). Ask me whether to install for this project or globally, and which agents I use. Then `cd senor && ./install --scope <my-answer> --agents <my-tools> --no-auto-update`.

### Non-interactive

```bash
./install --agents cursor,windsurf --scope project --no-auto-update --conflict skip
```

Run `./install --help` for the full flag list. To re-run after pulling new content, use `./update` (it reads the saved selections and reinstalls).

### Supported agents

Cursor, Windsurf, Cline, Codex CLI, GitHub Copilot, Claude Code (clone-only). For ChatGPT or anything else, point the agent at `SKILL.md` or a domain's `AGENTS.md` in a prompt.

---

## Prompt frameworks

These structures align with Señor's operating rules and reduce back-and-forth.

### Role, Context, Task, Output

The pattern recommended by [Anthropic's prompt engineering guide](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices).

```text
Role: Senior frontend engineer.
Context: Marketing site for a B2B SaaS targeting CFOs. Design tokens in src/styles/tokens.css.
Task: Draft the hero section in HTML/CSS.
Output: Single .html file, no JS, ≤ 60 lines.
```

### Pre-fill the brief

When a task is open-ended, Señor's rules tell the agent to ask for audience, purpose, scope, length, voice, and hard constraints. Volunteering them up front skips the asking round.

```text
Write a launch announcement for our new export-to-CSV feature.
- Audience: Existing customers, mostly data analysts.
- Purpose: Drive adoption in the next 30 days.
- Length: 400–600 words.
- Voice: Plain, technical, no marketing-deck adjectives.
- Hard constraints: Must mention rate limits and the new permission scope.
```

### Vague briefs get questions, not guesses

Señor's rules require asking for missing context before drafting. Vague briefs are fine.

```text
You: Write an article about Iceland.

Señor: A few questions before I draft.

1. Who's the audience?
2. What format? Blog, brochure, academic brief?
3. What length and voice?
4. What angle? Culture, geography, climate, history, food?

Answer what matters. I'll pick defaults for the rest.
```

### Anchor with examples

For style-heavy work (changelogs, blog posts, microcopy), paste one to three examples of the desired output. Examples beat description.

```text
Write a changelog entry in this style:

[2 prior entries pasted in full]

For: Added export-to-CSV in the reports tab.
```

---

## Tooling

The `scripts/` directory holds utilities for working with AI context. Right now the set is small and focused on maintaining this repository, but we'd like to grow it into more general-purpose territory that covers file conversion, scraping, validation, and the other work that goes into preparing reliable context for a model.

| Script                           | What it does                                                             |
| -------------------------------- | ------------------------------------------------------------------------ |
| `scripts/new-domain <name>`      | Scaffolds a new domain directory with a filled AGENTS.md template        |
| `scripts/check-sources [domain]` | Verifies every chapter file has a matching `.sources.md` citation file   |
| `scripts/format [--check]`       | Runs Prettier across all non-ignored files, with a `--check` mode for CI |

Scripts require Bash, and `format` additionally requires Node.js with `npx` available.

---

## Contributing

Contributions that improve the chapter files, add citations, or build out new domains are very welcome! Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request, since it covers file structure, voice rules, citation requirements, and how to validate that a rule change actually changes agent behavior.

The short version is that every new rule needs a human-authored source. AI-generated content (e.g. generated article) doesn't qualify as a citation and should not be used as a reference.

---

## License

Use these guidelines to build your own work, whether that's portfolios, client sites, apps, internal tools, or anything else, and whether it's commercial or not. Load them into AI agents on a per-session or per-project basis, drop them into Claude Code or Cursor, reference them in a system prompt while you build. The websites, apps, and software you produce are entirely yours.

You **may not** ship the guidelines as part of a commercial product. That means no repackaging or selling them as a standalone product, no bundling them into a paid SaaS feature or AI service or other commercial offering, and no embedding them as a permanent component of any distributed product or service. The same goes for using them as training data, fine-tuning data, or a system prompt that ships with your product.

Run a commercial product and want to incorporate these guidelines, whether it's an AI tool, a SaaS platform, a developer tool, or something else? **Get in touch and we'll work out an agreement!** Reach us at bert@masterbranchsoftware.com or via [masterbranchsoftware.com](https://masterbranchsoftware.com/contact).

One note on the integration recipes above. Pasting chapter files into your own project (e.g., `.cursor/rules/`, `.github/copilot-instructions.md`) is fine for internal use within your organization. If you publish or redistribute those copies in a public template repo, Section 7 of the LICENSE asks you to include the LICENSE terms with the copies and flag any modifications.

This project is licensed under the [Master Branch Source License v2.1](LICENSE). The LICENSE file is authoritative, and this section is just a plain-language summary for orientation.

This license was inspired by [CorridorKey](https://github.com/nikopueringer/CorridorKey) by Corridor Digital. Go check out their work and show them some support!

---

## Support

If these guidelines save you time and money, consider supporting continued development.

[![Support on Ko-fi](https://img.shields.io/badge/Ko--fi-Support-ff5f5f?style=for-the-badge&logo=kofi&logoColor=white)](https://ko-fi.com/masterbranch)
