# AGENTS.md

For AI coding agents (Claude Code, Cursor, Codex, ChatGPT, etc.) working in this repository. Read fully before any action.

## Goal

AI is getting good at coding but it's still a unrealiable. AI usually creates a mess from existing codebases and doesn't do good architectural decisions in new ones. AI doesnt follow good practices and doesn't do good architectural decisions. Security is also a concern, not just code.

We're trying to build comprehensive guidelines by analyzing tons of blog posts from real software professionals, official documentations, and professional grade codebases. We're hoping to close this gap and make LLMs more usable, more human. The code we're trying to generate should look human, have good architectural and security decisions. It shouldn't create unnecessary abstractions, over-engineer code, but find balance and create optimal solution.

First visible move from other creators is: https://github.com/forrestchang/andrej-karpathy-skills/blob/main/CLAUDE.md

We're trying to do something better. Something that isn't creating another friction layer but is solving the actual problem. Human interaction is of course still needed if not enough context is provided or something is wrong. It's still important to inform user about decisions. But AI should be more knowledgable. Write better code. Care about security. Not hallucinate and stay real.

## Rules

1. **Verify, don't assume.** Open files for paths, line numbers, function names, quotes, version numbers. Memory is not a source. Cannot verify in the moment? Mark inline ("uncited — verify before publishing") rather than fabricate.

2. **Cite human sources only.** Every new or changed rule in `<domain>/topic.md` requires a citation in `<domain>/topic.sources.md` in the same edit. Sources must be human-authored: official specs (W3C, WHATWG, ECMA), MDN, established engineering blogs, production codebases, books. AI-generated content does not qualify.

3. **Surface confusion and tradeoffs.** Don't paper over uncertainty. State alternatives when more than one answer is defensible. Name the trade-off, then recommend.

4. **Flag tool-vs-guideline conflicts.** When a recommended tool's default contradicts a written rule (e.g., Prettier emits `/>` on void elements while `front-end/javascript-and-html.md` forbids it), state the tension. Let the user decide; don't pick silently.

5. **Opinionated is fine if reasoned.** Document the reasoning inline. If the opinion contradicts authoritative documentation, name the contradiction and ask before overriding.

6. **Match existing voice and structure.** Reference voice: terse, imperative, no hedging ("Two-space indentation." not "Generally prefer two spaces."). No emojis. Chapter files (`<domain>/topic.md`) are curated — extend existing chapters; new chapters or top-level files require user approval.

7. **Compress for LLM reading.** Chapters are loaded into agent context on every task; every saved token is leverage. Write to be parsed, not enjoyed.

   Compression hierarchy — apply most aggressively where load frequency is highest:
   - `<domain>/AGENTS.md` (loaded every task in the domain) — tightest. No prose paragraphs > 2 sentences.
   - `<domain>/topic.md` (loaded only when the task touches the chapter) — tight, but must read standalone.
   - `<domain>/*.sources.md` (developer-only, never loaded by consumers) — lowest priority.

   Cut these:
   - Warm-up first sentences ("In this chapter we will explore …", "It is important to understand …"). Lead with the rule.
   - Transitional and meta-commentary phrases ("It is worth noting", "Furthermore", "As mentioned above").
   - Restatement. Two consecutive sentences carrying the same information → delete one.
   - Conjoined claims. One claim, one sentence; split.
   - Worked examples that don't teach beyond the rule. Two examples making the same point → keep the strongest.
   - Prose where order doesn't carry reasoning. Convert to bullets or a table.

   Preserve (do NOT cut):
   - Non-negotiables — every bullet.
   - Citations and canonical names inline (Joos 1962, Diátaxis, IMRaD, NN/g, named style guides). They anchor authority.
   - Worked examples that _are_ the rule — bad/good pairs, before/after, register-shift demonstrations. The example carries information the rule alone does not.
   - Multi-dimensional tables. Rows that look formulaic usually encode different data per row.
   - Inline content the chapter needs to make sense standalone, even if it duplicates content elsewhere.

   Cross-reference vs inline:
   - Cross-reference for _deeper detail_ the reader can opt into (full lists, full procedures, related chapters).
   - Keep _inline_ the content the current chapter's argument relies on. A chapter that reads as a stub of pointers has lost data, not compressed it.
   - When extending an existing chapter, prefer compressing surrounding prose to make room rather than appending.

   Trim to the smallest form that preserves the information. If a sentence can be removed without an agent losing knowledge, remove it. If removing it forces the next agent to load another file to understand the current paragraph, leave it.

8. **Replace, don't layer.** Edit rules in place. No "we previously said X, now Y" phrasing. Git history carries the audit trail.

9. **Touch only what the task requires.** No drive-by refactors. Clean up only what the current change introduces. Files in `.prettierignore` (e.g., `web-design/index.html`) stay hand-formatted unless the user says otherwise.

10. **Validate material guidance via evals.** New or changed rules in any chapter belong in `<domain>/workspace/iteration-N/` as a WITH-skill vs WITHOUT-skill comparison. Guidance that doesn't change agent behavior is dead weight.

11. **Keep sources current.** When a rule changes, update its sidecar in the same commit. When a cited URL rots, replace or remove. When a tool-catalog entry in a sidecar becomes load-bearing for a chapter rule, add a primary citation alongside it.

12. **Ask before breaking a rule.** Any of the above can be wrong in a specific case. Surface the case, propose the deviation, wait for the user.

13. **Don't rely on memory, always search online.** Look through articles from professionals, documentation, professional grade codebases. Doubt your knowledge, always verify. Follow our goal.

## Project layout

```
GOAL.md                              Premise.
README.md                            Public-facing description.
AGENTS.md                            This file (developer rules for working in this repo).
SKILL.md                             Cross-domain operating rules for AI skill consumers.
CONTRIBUTING.md                      Contributor guide — structure, conventions, how to add a domain.
LICENSE                              License terms.
scripts/
  new-domain                         Scaffold a new skill domain directory.
  check-sources                      Verify every chapter has a matching sources file.
  format                             Run Prettier across all non-ignored files.
.prettierrc.json                     Formatter config (per `front-end/code-style-and-quality.md`).
.prettierignore                      Hand-formatted files, eval outputs, standard exclusions.

front-end/                           Web design, HTML/CSS/JS/TS, web copy, a11y, performance.
  AGENTS.md                          Skill entry point. Loaded by skill consumers.
  index.html                         Landing page. Hand-formatted; in .prettierignore.
  topic.md                           Chapter (loaded by skill consumers).
  topic.sources.md                   Citations + curated tool catalogs for that chapter (developer-only).
  inspiration-gallery.md             Curated design references by feel and category.
  evals/                             Eval prompt definitions.
  workspace/
    iteration-N/                     Eval runs (recorded outputs; in .prettierignore).

copywriting/                         Cross-genre prose — register, voice, blogs, marketing, technical, research.
  AGENTS.md                          Skill entry point.
  topic.md                           Chapter (loaded by skill consumers).
  topic.sources.md                   Citations for that chapter (developer-only).

security/                            Security audits, hardening, threat modeling.
  AGENTS.md                          Skill entry point (stub — in progress).

ruby/                                Ruby language, idioms, best practices.
  AGENTS.md                          Skill entry point (stub — in progress).
  rails/                             Rails-specific guidelines (planned).

architecture/                        Cross-stack architectural patterns and decisions.
  AGENTS.md                          Skill entry point (stub — in progress).

documentation/                       READMEs, contributing guides, changelogs, architecture docs.
  AGENTS.md                          Skill entry point.
  topic.md                           Chapter (loaded by skill consumers).
  topic.sources.md                   Citations for that chapter (developer-only).
```

## Two audiences, two file sets

- **Skill consumers** (agents helping users) load root `SKILL.md`, then the relevant domain `AGENTS.md`, then chapter files (`<domain>/topic.md`), then `SUMMARY.md` on demand. They do not load `*.sources.md`.
- **Developers** (agents and humans editing this repo) load `AGENTS.md`, edit chapters and sidecars together, run evals to validate. When working in this repo, you are a developer.

## Tooling

- **Prettier** is the configured formatter (`.prettierrc.json`). Run `npx prettier --write <file>` on any file you edit. Existing files were authored before this config and have not been bulk-formatted; format them on next edit, not pre-emptively.
- **Eval harness** lives in `web-design-workspace/`. New rules need a paired eval comparing WITH-skill vs WITHOUT-skill output. Recorded responses are not formatted.

## When in doubt

Ask. The cost of a question is one round-trip. The cost of a wrong assumption is silently broken guidance the agent will trust forever.
