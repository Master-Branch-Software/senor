# Skill Guidelines

Cross-domain operating rules for every AI agent using skills in this repository.
Load this file before any domain skill when context allows.

## Repository layout

Skills are organized by domain. Each domain has an `AGENTS.md` that routes to chapter files.

| Domain        | Entry point               | Covers                                                                            |
| ------------- | ------------------------- | --------------------------------------------------------------------------------- |
| Front-end     | `front-end/AGENTS.md`     | Web design, HTML/CSS/JS/TS, web copy, a11y, performance, stack selection          |
| Copywriting   | `copywriting/AGENTS.md`   | Cross-genre prose: register, voice, blogs, marketing, technical writing, research |
| Security      | `security/AGENTS.md`      | Security audits, pen testing, threat modeling, hardening                          |
| Ruby          | `ruby/AGENTS.md`          | Ruby language, idioms, best practices                                             |
| Architecture  | `architecture/AGENTS.md`  | Software architecture patterns and decisions                                      |
| Documentation | `documentation/AGENTS.md` | READMEs, contributing guides, changelogs, architecture docs                       |

## Operating principles

These apply across all skills in this repository.

1. Read the primary chapter before proposing any solution touching it. For tasks that build from scratch: read every chapter in the task row before producing any artifact — open the file, don't rely on memory.
2. Apply rules silently; never paraphrase back into output.
3. Ask only when the answer changes the output.
4. Written artifacts before code. Text reveals unclear thinking; code hides it.
5. End every artifact with a one-line self-review naming deviations. Run any domain-defined checklists before submitting.
6. Later stage never silently overwrites earlier artifact. Fix upstream flaws upstream.

## When in doubt

Favor the simplest thing that works, uses the platform, respects the user, and reads well six months from now.
