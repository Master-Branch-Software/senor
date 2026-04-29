# Contributing

This document explains the structure so you can add or improve content without breaking how agents consume it.

## Repository structure

```
SKILL.md               Cross-domain operating rules (read by AI agents before any domain skill).
AGENTS.md              Developer rules for working in this repo (read this file and AGENTS.md first).

<domain>/
  AGENTS.md            Skill entry point — task routing, non-negotiables, running order.
  topic.md             Chapter file — the actual guidelines.
  topic.sources.md     Citations backing that chapter (developer artifact, not loaded by agents).
  evals/               Eval prompt definitions.
  workspace/           Recorded eval runs (never formatted, listed in .prettierignore).
```

Example domains: `front-end/`, `copywriting/`, `documentation/`, `security/`, `ruby/`, `architecture/`.

## Two file audiences

Every file in this repo is written for one of two audiences. Keep them separate.

| Audience                                                | Files                                                                    | Purpose                                                  |
| ------------------------------------------------------- | ------------------------------------------------------------------------ | -------------------------------------------------------- |
| **Skill consumers** — AI agents                         | `SKILL.md`, `<domain>/AGENTS.md`, `<domain>/topic.md`                    | Loaded at task time to guide agent behavior              |
| **Developers** — humans and agents working in this repo | `AGENTS.md`, `<domain>/topic.sources.md`, `CONTRIBUTING.md`, and similar | Editorial and process tooling, never loaded by consumers |

## Branching model

- `master` — the working branch. All PRs target this branch.

To contribute:

1. Fork the repository.
2. Create a feature branch from `master` in your fork (`git checkout -b my-change master`).
3. Make your changes and push to your fork.
4. Open a pull request from your fork's feature branch into `master` on the main repo.

## Adding content to an existing domain

1. **Find the right chapter.** Check `<domain>/AGENTS.md` for the task-to-chapter map. Extend an existing chapter before creating a new one.
2. **Edit the chapter file.** Follow the voice in [AGENTS.md](AGENTS.md) rule 6, which means terse, imperative, no hedging. One claim, one sentence.
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
| 1     | YAML frontmatter                   | yes                                              | `name`, `description`. See [frontmatter rules](#frontmatter-rules).                                                                                               |
| 2     | `# <Domain> Guide`                 | yes                                              | H1 title only.                                                                                                                                                    |
| 3     | Pointer to `SKILL.md`              | yes                                              | One sentence: "General operating principles live in `SKILL.md` at the repository root. Read it first."                                                            |
| 4     | `## Non-negotiables`               | yes                                              | Bulleted absolute minimums. One rule per bullet. No category headers within the list.                                                                             |
| 5     | `## Task-to-chapter map`           | yes                                              | Three-column table: Task / Primary / Also consider. One row per likely user intent. Cells are chapter filenames (`topic.md`) or paths to other domains' chapters. |
| 6     | `## Domain checklists`             | yes once chapters exist                          | Bullets pointing to specific chapter sections (`topic.md` § Section). Run before submitting any artifact.                                                         |
| 7     | `## Running order for <task type>` | when the domain has a multi-stage workflow       | Numbered list. Each step names an artifact and what to confirm.                                                                                                   |
| 8     | `## Context shape`                 | when the domain has substantial inputs to gather | Bullets of facts to confirm before starting. The agent asks for missing items.                                                                                    |
| 9     | `## Skill connections`             | yes                                              | Bullets: `<other skill or path> → <when to reach for it>`. Includes both in-repo (`<domain>/AGENTS.md`) and external (if installed) skills.                       |

### Frontmatter rules

```yaml
---
name: <domain> # exactly the directory name, lowercase, kebab-case
description: |
  Multi-sentence trigger description. Specific scope, common task verbs,
  and "even if the user does not say X explicitly" disambiguators. Used
  by skill-loading systems to score relevance — write for that scoring,
  not for human readers. End with cross-skill pointers when the domain
  pairs naturally with another (e.g., front-end ↔ copywriting).
---
```

- `name` matches directory name verbatim. No alternate forms.
- `description` is loaded into every agent's prompt, so spend the tokens on disambiguation rather than marketing.
- Mention surfaces and verbs the agent will see in user requests ("hero section", "audit", "writing", "harden").
- Name the negative disambiguators when the domain is easy to confuse with a sibling ("for README structure read `documentation/AGENTS.md`").

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

See `AGENTS.md` rule 7 for the full set (compression hierarchy, what to cut, what to preserve, cross-reference vs inline). The same rules apply to chapter files, and this section adds chapter-specific procedure on top.

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
- Frontmatter `description` text used by skill loaders for relevance scoring.

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

When adding a new domain, three files need a routing edit. Each one is a localized insert, so do not reorganize the surrounding content.

### 1. Root `SKILL.md` — domain table

Add a row to the routing table. Keep alphabetical or grouped order if the existing table follows one.

```diff
 | Domain        | Entry point               | Covers                                                                    |
 | ------------- | ------------------------- | ------------------------------------------------------------------------- |
 | Front-end     | `front-end/AGENTS.md`     | Web design, HTML/CSS/JS/TS, web copy, a11y, performance, stack selection  |
+| <Domain>      | `<domain>/AGENTS.md`      | <one-line scope — same wording as the frontmatter, ≤ 80 chars>            |
 | Security      | `security/AGENTS.md`      | Security audits, pen testing, threat modeling, hardening                  |
```

The `Covers` cell is the disambiguator agents read first, so spend the tokens on scope rather than marketing.

### 2. Root `AGENTS.md` — project layout

Add a block to the project-layout code fence. Match the indentation and line-up of existing blocks.

```diff
 front-end/                           Web design, HTML/CSS/JS/TS, web copy, a11y, performance.
   AGENTS.md                          Skill entry point. Loaded by skill consumers.
   ...

+<domain>/                            <one-line scope, same as SKILL.md row>.
+  AGENTS.md                          Skill entry point.
+  topic.md                           Chapter (loaded by skill consumers).
+  topic.sources.md                   Citations for that chapter (developer-only).
```

### 3. `CONTRIBUTING.md` — current domains list

Add the new domain to the inline list.

```diff
-Current domains: `front-end/`, `copywriting/`, `documentation/`, `security/`, `ruby/`, `architecture/`.
+Current domains: `front-end/`, `copywriting/`, `documentation/`, `<domain>/`, `security/`, `ruby/`, `architecture/`.
```

### 4. Cross-skill pointers (when applicable)

If the new domain pairs with an existing one, add a `Skill connections` bullet to that domain's `AGENTS.md` so agents working there know to load yours too. Make the pointers symmetric (both directions) when the pairing is genuine.

## Eval convention

Evals live in `<domain>/workspace/iteration-N/`. Each eval compares WITH-skill vs WITHOUT-skill agent output on a representative task. The goal is to prove that a rule change actually changes agent behavior — not just reads well.

Record raw agent output unformatted, alongside a `summary.md` describing what changed and why.

## What not to do

- Do not create a new chapter when an existing one can be extended.
- Do not write AI-generated content as a source citation.
- Do not add a rule without a citation in the matching `*.sources.md`.
- Do not format files in `.prettierignore` — they are intentionally hand-formatted or artifact data.
- Do not edit `SKILL.md` (root) or `AGENTS.md` (root) for domain-specific content — those are cross-cutting files.
