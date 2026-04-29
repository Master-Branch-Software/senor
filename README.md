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
├── scripts/                          ← utilities for working with AI context
├── CONTRIBUTING.md                   ← how to add a domain, chapter, or citation
└── LICENSE
```

---

## How to use

Clone the repository into your project's skill directory, compatible agents pick it up automatically.

```bash
# As a git submodule (recommended — keeps it updatable)
git submodule add https://github.com/Master-Branch-Software/ai-web-designer .claude/skills/ai-web-designer

# Or a plain clone
git clone https://github.com/Master-Branch-Software/ai-web-designer .claude/skills/ai-web-designer
```

The agent reads `SKILL.md` for cross-domain rules, then loads the relevant `AGENTS.md` when the task matches a domain.

| Tool                        | Integration                                                                        |
| --------------------------- | ---------------------------------------------------------------------------------- |
| **Claude Code**             | Clone into `.claude/skills/`, auto-discovered from SKILL.md frontmatter            |
| **Windsurf**                | Clone into your skills directory, same SKILL.md convention                         |
| **Cursor**                  | Drop `front-end/AGENTS.md` into `.cursor/rules/` as a `.mdc` file                  |
| **GitHub Copilot**          | Copy `front-end/AGENTS.md` into `.github/copilot-instructions.md`                  |
| **ChatGPT**                 | Attach the files to a project, or paste relevant chapters into custom instructions |
| **Cline**                   | Reference the repo path in your project rules                                      |
| **Any other AI**            | Point the agent at `SKILL.md` or the relevant `AGENTS.md` in a prompt              |

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
