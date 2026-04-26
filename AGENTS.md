# AGENTS.md

For AI coding agents (Claude Code, Cursor, Codex, ChatGPT, etc.) working in this repository. Read fully before any action.

## Goal

Comprehensive guidelines for LLM-assisted web development — frontend, copy, security, architecture — so agent output reads as human, sound, and right-sized. No over-engineering. No hallucination. No junior-grade code. Full premise: `GOAL.md`.

## Rules

1. **Verify, don't assume.** Open files for paths, line numbers, function names, quotes, version numbers. Memory is not a source. Cannot verify in the moment? Mark inline ("uncited — verify before publishing") rather than fabricate.

2. **Cite human sources only.** Every new or changed rule in `references/NN-topic.md` requires a citation in `references/NN-topic.sources.md` in the same edit. Sources must be human-authored: official specs (W3C, WHATWG, ECMA), MDN, established engineering blogs, production codebases, books. AI-generated content does not qualify.

3. **Surface confusion and tradeoffs.** Don't paper over uncertainty. State alternatives when more than one answer is defensible. Name the trade-off, then recommend.

4. **Flag tool-vs-guideline conflicts.** When a recommended tool's default contradicts a written rule (e.g., Prettier emits `/>` on void elements while chapter 07 forbids it), state the tension. Let the user decide; don't pick silently.

5. **Opinionated is fine if reasoned.** Document the reasoning inline. If the opinion contradicts authoritative documentation, name the contradiction and ask before overriding.

6. **Match existing voice and structure.** Reference voice: terse, imperative, no hedging ("Two-space indentation." not "Generally prefer two spaces."). No emojis. Numbered chapter files (`web-design/references/NN-topic.md`) are curated — extend existing chapters; new chapters or top-level files require user approval.

7. **Compress for LLM reading.** Drop fluff and restatement. Token count is a budget. Chapters are read by agents at every task — every saved token is leverage. Trim to the smallest form that preserves information.

8. **Replace, don't layer.** Edit rules in place. No "we previously said X, now Y" phrasing. Git history carries the audit trail.

9. **Touch only what the task requires.** No drive-by refactors. Clean up only what the current change introduces. Files in `.prettierignore` (e.g., `web-design/index.html`) stay hand-formatted unless the user says otherwise.

10. **Validate material guidance via evals.** New or changed rules in `references/` belong in `web-design-workspace/iteration-N/` as a WITH-skill vs WITHOUT-skill comparison. Guidance that doesn't change agent behavior is dead weight.

11. **Keep sources current.** When a rule changes, update its sidecar in the same commit. When a cited URL rots, replace or remove. When a tool-catalog entry in a sidecar becomes load-bearing for a chapter rule, add a primary citation alongside it.

12. **Ask before breaking a rule.** Any of the above can be wrong in a specific case. Surface the case, propose the deviation, wait for the user.

## Project layout

```
GOAL.md                              Premise.
README.md                            Public-facing description.
AGENTS.md                            This file.
.prettierrc.json                     Formatter config (per chapter 15).
.prettierignore                      index.html, eval outputs, standard exclusions.

web-design/
  SKILL.md                           Skill entry point. Loaded by skill consumers.
  index.html                         Landing page. Hand-formatted; in .prettierignore.
  evals/                             Eval prompt definitions.
  references/
    NN-topic.md                      Chapter (loaded by skill consumers; NN runs 01–15).
    NN-topic.sources.md              Citations + curated tool catalogs for that chapter (developer-only).
    SUMMARY.md, USAGE.md             Cross-cutting docs.

web-design-workspace/
  iteration-N/                       Eval runs (recorded outputs; in .prettierignore).
```

## Two audiences, two file sets

- **Skill consumers** (agents helping users build websites) load `SKILL.md`, then chapter files (`NN-topic.md`), then `SUMMARY.md` / `USAGE.md` on demand. They do not load `*.sources.md`.
- **Developers** (agents and humans editing this repo) load `AGENTS.md`, edit chapters and sidecars together, run evals to validate. When working in this repo, you are a developer.

## Tooling

- **Prettier** is the configured formatter (`.prettierrc.json`). Run `npx prettier --write <file>` on any file you edit. Existing files were authored before this config and have not been bulk-formatted; format them on next edit, not pre-emptively.
- **Eval harness** lives in `web-design-workspace/`. New rules need a paired eval comparing WITH-skill vs WITHOUT-skill output. Recorded responses are not formatted.

## When in doubt

Ask. The cost of a question is one round-trip. The cost of a wrong assumption is silently broken guidance the agent will trust forever.
