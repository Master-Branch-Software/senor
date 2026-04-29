# Contributing

This document explains the structure so you can add or improve content without breaking how agents consume it. When working in this repo (editing chapters, writing rules, adding domains), you are a developer. Read this file in full before any action.

## Repository structure

```
SKILL.md               Cross-domain operating rules (loaded by AI agents before any domain skill).
CONTRIBUTING.md        Developer rules, conventions, and contribution procedure (this file).
README.md              Public-facing project description.
LICENSE                License terms.
install                Interactive installer that writes rule files for non-Claude agents.
update                 Non-interactive `git pull` + reinstall used by the auto-update task.
templates/             Single source of truth for rule-file content. `__SENOR_SKILL__` is replaced at install time:
                       - `./install` substitutes the absolute path to SKILL.md (for installed users).
                       - `scripts/sync-rules` substitutes `./SKILL.md` and writes the root contributor files
                         (`AGENTS.md`, `.cursor/rules/senor.mdc`, etc.). Re-run after editing a template.
.prettierrc.json       Formatter config.
.prettierignore        Hand-formatted files, eval outputs, standard exclusions.
scripts/               Repo utilities (`new-domain`, `check-sources`, `format`).

<domain>/
  AGENTS.md            Skill entry point — task routing, non-negotiables, running order.
  topic.md             Chapter file — the actual guidelines.
  topic.sources.md     Citations backing that chapter (developer artifact, not loaded by agents).
  evals/               Eval prompt definitions.
  workspace/           Recorded eval runs (never formatted, listed in .prettierignore).
```

Current domains: `front-end/`, `copywriting/`, `documentation/`, `security/`, `ruby/`, `architecture/`.

## Two file audiences

Every file in this repo is written for one of two audiences. Keep them separate.

| Audience                                                | Files                                                       | Purpose                                                  |
| ------------------------------------------------------- | ----------------------------------------------------------- | -------------------------------------------------------- |
| **Skill consumers** — AI agents                         | `SKILL.md`, `<domain>/AGENTS.md`, `<domain>/topic.md`       | Loaded at task time to guide agent behavior              |
| **Developers** — humans and agents working in this repo | `CONTRIBUTING.md`, `<domain>/topic.sources.md`, and similar | Editorial and process tooling, never loaded by consumers |

## Working principles

These apply to every edit in the repo.

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

13. **Don't rely on memory, always search online.** Look through articles from professionals, documentation, professional-grade codebases. Doubt your knowledge, always verify.

## Tooling

- **Prettier** is the configured formatter (`.prettierrc.json`). Run `npx prettier --write <file>` on any file you edit. Existing files were authored before this config and have not been bulk-formatted; format them on next edit, not pre-emptively.
- **Scripts** in `scripts/`:
  - `scripts/new-domain <name>` — scaffold a new domain directory.
  - `scripts/check-sources [domain]` — verify every chapter has a matching sources file.
  - `scripts/format [--check]` — run Prettier across non-ignored files.
  - `scripts/sync-rules [--check]` — regenerate the root contributor rule files (`.cursor/rules/senor.mdc`, `AGENTS.md`, etc.) from `templates/`. Run after editing anything in `templates/`. `--check` mode reports drift without writing — wire into CI.
- **Eval harness** lives in `<domain>/workspace/iteration-N/`. New rules need a paired eval comparing WITH-skill vs WITHOUT-skill output. Recorded responses are not formatted.

## Branching model

- `master` — the working branch. All PRs target this branch.

To contribute:

1. Fork the repository.
2. Create a feature branch from `master` in your fork (`git checkout -b my-change master`).
3. Make your changes and push to your fork.
4. Open a pull request from your fork's feature branch into `master` on the main repo.

## Adding content to an existing domain

1. **Find the right chapter.** Check `<domain>/AGENTS.md` for the task-to-chapter map. Extend an existing chapter before creating a new one.
2. **Edit the chapter file.** Follow the voice in [§ Working principles, rule 6](#working-principles) — terse, imperative, no hedging. One claim, one sentence.
3. **Add a citation.** Every new or changed rule needs a source in the matching `topic.sources.md`. Use human-authored sources only, drawn from official specs, MDN, established engineering blogs, production codebases, and books. No AI-generated content.
4. **Run an eval.** New or significantly changed rules belong in `workspace/iteration-N/` as a WITH-skill vs WITHOUT-skill comparison. Guidance that doesn't change agent behavior is dead weight.
5. **Run Prettier.** `npx prettier --write <file>` on every file you touch except those in `.prettierignore`.

## Adding a new domain

1. Scaffold the directory: `./scripts/new-domain <domain>` (creates `<domain>/` and a stub `AGENTS.md`).
2. Fill `<domain>/AGENTS.md` per the [domain entry-point structure](#domain-entry-point-structure).
3. Add chapter files (`<domain>/topic.md` + `<domain>/topic.sources.md`) directly under the domain folder once you have content to write.
4. Wire routing per [Routing edits](#routing-edits).

## Domain entry-point structure

`<domain>/AGENTS.md` is the file skill consumers load first. It is a contract between the domain and the agent. Sections appear in the order below, and required sections must be present.

| Order | Section                            | Required                                         | Contents                                                                                                                                                          |
| ----- | ---------------------------------- | ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | `# <Domain> Guide`                 | yes                                              | H1 title only.                                                                                                                                                    |
| 2     | Pointer to `SKILL.md`              | yes                                              | One sentence: "General operating principles live in `SKILL.md` at the repository root. Read it first."                                                            |
| 3     | `## Non-negotiables`               | yes                                              | Bulleted absolute minimums. One rule per bullet. No category headers within the list.                                                                             |
| 4     | `## Task-to-chapter map`           | yes                                              | Three-column table: Task / Primary / Also consider. One row per likely user intent. Cells are chapter filenames (`topic.md`) or paths to other domains' chapters. |
| 5     | `## Domain checklists`             | yes once chapters exist                          | Bullets pointing to specific chapter sections (`topic.md` § Section). Run before submitting any artifact.                                                         |
| 6     | `## Running order for <task type>` | when the domain has a multi-stage workflow       | Numbered list. Each step names an artifact and what to confirm.                                                                                                   |
| 7     | `## Context shape`                 | when the domain has substantial inputs to gather | Bullets of facts to confirm before starting. The agent asks for missing items.                                                                                    |
| 8     | `## Skill connections`             | yes                                              | Bullets: `<other skill or path> → <when to reach for it>`. Includes both in-repo (`<domain>/AGENTS.md`) and external (if installed) skills.                       |

Routing across domains is carried by root `SKILL.md`'s domain table. Per-domain entry points do not carry YAML frontmatter — anything an external skill loader would read from frontmatter belongs in `SKILL.md` instead.

### Section conventions

- **Voice** — terse, imperative, no hedging. Same rules as chapter files.
- **Cross-references** — always write `<path>/<file>.md § <Section>` so agents can jump directly. Bare filenames are ambiguous when a section header is what's meant.
- **Length** — entry points should fit in ~150 lines. Detail belongs in chapter files.
- **No prose blocks** longer than two sentences. If you need a paragraph, the chapter file probably needs the content.

## Chapter file structure

Chapter files (`<domain>/topic.md`) carry the actual guidelines.

- **Naming** — short, lowercase, hyphenated topic slug (e.g. `accessibility.md`, `voice-and-perspective.md`). No numeric prefix — file order is reading order in the directory listing only; the task-to-chapter map in `<domain>/AGENTS.md` carries any precedence the agent needs.
- **Curation** — chapter files are curated. New chapters require maintainer approval, so extend an existing chapter first.
- **Required sidecar** — every `topic.md` has a paired `topic.sources.md` in the same commit. CI (`./scripts/check-sources <domain>`) enforces.
- **Voice** — terse, imperative, no hedging ("Two-space indentation." not "Generally prefer two spaces."). No emojis. No conversational filler.

### Required structure

| Order | Element                            | Notes                                                                                         |
| ----- | ---------------------------------- | --------------------------------------------------------------------------------------------- |
| 1     | `# Title`                          | Sentence-case noun phrase. Matches the file slug.                                             |
| 2     | One-paragraph orientation          | What the chapter covers, what it does not, where related material lives. ≤ 3 sentences.       |
| 3     | `## Section` headers               | Each names a single concept. Sub-sections (`###`) only when a section has 4+ named sub-rules. |
| 4     | Tables, lists, and worked examples | Tables and lists are the default, with prose used only when sequence matters.                 |
| 5     | Cross-references                   | Inline by path and section header.                                                            |

### Compression rules

See [§ Working principles, rule 7](#working-principles) for the full set (compression hierarchy, what to cut, what to preserve, cross-reference vs inline). The same rules apply to chapter files; this section adds chapter-specific procedure on top.

### Tightening an existing chapter or domain

Use this procedure when the task is to compress a chapter or a domain that already exists. It does not apply when you're adding new material.

#### Order of passes

Apply the passes in this order, and let each pass do its one job without blending in work from another.

1. **Read end-to-end first.** Note structure, repeated patterns, prose-vs-list balance, where cross-references already live. Compression without first reading produces uneven cuts.
2. **Cut warm-up sentences and meta-commentary.** Paragraph leads that announce ("In this section we …", "It is worth noting …"). Lowest-risk cut.
3. **Convert prose to bullets or tables** where order does not carry reasoning. A three-sentence prose paragraph listing three things → three bullets.
4. **Collapse restatement.** Two consecutive sentences carrying the same information → delete the weaker one.
5. **Split conjoined claims.** One sentence with `and`/`which` doing structural work → two sentences.
6. **Audit worked examples.** Two examples making the same point → keep the strongest. Examples that _are_ the rule (bad/good pairs, before/after) stay.
7. **Tighten the entry point hardest.** `<domain>/AGENTS.md` is loaded every task, so keep bullets to ≤ 1 sentence and no prose blocks longer than 2 sentences.

Do not run a cross-chapter dedupe pass without explicit user approval. The same content appearing in 2–3 chapters is usually contextualized for each chapter's reader, and replacing inline content with cross-references is data loss disguised as compression. There is one exception, which is identical multi-paragraph blocks between two chapters where one is genuinely a duplicate. Even then, ask first.

#### What to preserve

- Non-negotiables — every bullet.
- Citations and canonical names inline (Joos 1962, Diátaxis, IMRaD, NN/g, Strunk & White, Pinker).
- Worked examples that demonstrate the rule by being it (bad/good headline pairs, register-shift examples, before/after edits).
- Multi-dimensional tables — rows that look formulaic usually encode different data per row.
- Inline content the chapter needs to read standalone, even if duplicated in another chapter.

#### Stopping criteria

Stop when any of these is true:

- Next pass would drop below ~5% reduction. Diminishing returns.
- Chapter starts reading like a checklist instead of guidance — restore some prose where the reasoning needs to flow.
- Cross-references replace inline content the chapter needs to make sense alone — restore inline.
- Worked examples have been pruned past the point where the rule alone teaches the same thing — restore the example.
- A typical good pass on a chapter of 200+ lines lands at 15–20% reduction with no rule, citation, or load-bearing example lost.

#### Verification

After tightening:

- Run `npx prettier --write` on every modified file.
- Run `./scripts/check-sources <domain>` — chapter/source pairing must still pass.
- Re-read each modified chapter end-to-end as if encountering it cold. If any rule no longer makes sense without loading another file, restore the inline content.
- Diff line count before/after. Report the reduction per file in the commit message.

## Sources sidecar structure

`<domain>/topic.sources.md` is developer-facing. Skill consumers do not load it.

| Order | Section                   | Required        | Contents                                                                                                                                                                                      |
| ----- | ------------------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | `# Sources — <topic>.md`  | yes             | Matches the chapter filename.                                                                                                                                                                 |
| 2     | `## Primary citations`    | yes             | Bulleted list. Every entry carries an author, title, publisher/year, URL where available, and a one-sentence note on what the source backs in the chapter.                                    |
| 3     | `## Secondary references` | when applicable | Same format as primary. Use for sources that strengthen but are not load-bearing.                                                                                                             |
| 4     | `## Curated tool catalog` | when applicable | Tools the chapter recommends or implies. Every entry carries a name, a one-line purpose, and a link. Tool entries that become load-bearing for a rule must be promoted to a primary citation. |
| 5     | `## Notes for editors`    | when applicable | Editorial guidance for future contributors, covering what's stable, what to verify before extending, and any naming or synthesis decisions.                                                   |

Source rules:

- Human-authored only. Official specs (W3C, WHATWG, ECMA), MDN, established engineering blogs, named books, peer-reviewed papers, production codebases. No AI-generated content.
- Every new or changed rule in the chapter requires a citation here in the same edit.
- When a cited URL rots, replace or remove. When a tool-catalog entry becomes load-bearing for a chapter rule, add a primary citation alongside it.

## Routing edits

When adding a new domain, two files need a routing edit. Each one is a localized insert, so do not reorganize the surrounding content.

### 1. Root `SKILL.md` — domain table

Add a row to the routing table. Keep alphabetical or grouped order if the existing table follows one.

```diff
 | Domain        | Entry point               | Covers                                                                    |
 | ------------- | ------------------------- | ------------------------------------------------------------------------- |
 | Front-end     | `front-end/AGENTS.md`     | Web design, HTML/CSS/JS/TS, web copy, a11y, performance, stack selection  |
+| <Domain>      | `<domain>/AGENTS.md`      | <one-line scope — concrete subjects, ≤ 80 chars>                          |
 | Security      | `security/AGENTS.md`      | Security audits, pen testing, threat modeling, hardening                  |
```

The `Covers` cell is the disambiguator agents read first, so spend the tokens on scope rather than marketing.

### 2. `CONTRIBUTING.md` — current domains list

Add the new domain to the inline list at [§ Repository structure](#repository-structure).

```diff
-Current domains: `front-end/`, `copywriting/`, `documentation/`, `security/`, `ruby/`, `architecture/`.
+Current domains: `front-end/`, `copywriting/`, `documentation/`, `<domain>/`, `security/`, `ruby/`, `architecture/`.
```

### 3. Cross-skill pointers (when applicable)

If the new domain pairs with an existing one, add a `Skill connections` bullet to that domain's `AGENTS.md` so agents working there know to load yours too. Make the pointers symmetric (both directions) when the pairing is genuine.

## Eval convention

Evals live in `<domain>/workspace/iteration-N/`. Each eval compares WITH-skill vs WITHOUT-skill agent output on a representative task. The goal is to prove that a rule change actually changes agent behavior — not just reads well.

Record raw agent output unformatted, alongside a `summary.md` describing what changed and why.

## What not to do

- Do not create a new chapter when an existing one can be extended.
- Do not write AI-generated content as a source citation.
- Do not add a rule without a citation in the matching `*.sources.md`.
- Do not format files in `.prettierignore` — they are intentionally hand-formatted or artifact data.
- Do not edit `SKILL.md` (root) for domain-specific content — it is cross-cutting.
