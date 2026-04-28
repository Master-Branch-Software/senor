# Technical Writing

Documentation for software, hardware, any system the reader will operate. Audience reads to _do_ something. Every craft choice serves the doing.

For READMEs and project-doc structure, read `documentation/AGENTS.md`. This chapter covers technical voice, doc-type selection, and patterns spanning every technical artifact.

## Four document types (Diátaxis)

Procida's framework. Mixing types is the most common cause of confusing docs.

| Type        | Need               | Posture      | Reader is                  | Test                                   |
| ----------- | ------------------ | ------------ | -------------------------- | -------------------------------------- |
| Tutorial    | Learning           | Teacher      | Beginner                   | Did the reader complete the lesson?    |
| How-to      | Goal-completion    | Mentor       | Competent user with a task | Did they achieve the goal?             |
| Reference   | Information lookup | Encyclopedia | User with a known need     | Was the fact accurate and findable?    |
| Explanation | Understanding      | Discussant   | Curious user               | Did the reader's mental model improve? |

Three rules:

1. Pick a type per page. A page that opens as tutorial and ends as reference is two pages.
2. Cross-link across types.
3. Voice differs by type.

### Tutorial voice

Second person, present tense, active. "You install the package."

- Promise the outcome at the top.
- Number every step. Smallest action that produces a visible result.
- Show output after every step.
- Prerequisites short and explicit. A tutorial that fails on a missing prerequisite is broken.
- Do not branch (`if macOS … if Linux … if Windows`) — that's three tutorials.

### How-to voice

Second person, imperative. "Configure the database."

- Title is the goal as a task: "Migrate from version 2 to 3."
- Open with one-sentence summary of when this guide applies.
- Steps as imperative verbs. "Stop the service. Run the migration. Verify the schema. Start the service."
- Conditional steps explicit.
- End with a verification step.

### Reference voice

Third-person impersonal or no person. Terse. Complete.

- Every entry stands alone — reader arrives from search.
- Same structure for every entry of the same kind (Parameters, Returns, Raises, Example).
- No editorializing.
- Examples are runnable. A reference example that doesn't work is worse than none.

### Explanation voice

First-person plural or impersonal. Reflective. Allowed a viewpoint.

- Title names the question: "Why we chose Postgres over MySQL."
- Argues a position; reader expects it.
- Compare alternatives honestly.
- Cross-link out at the end.

## Cross-type principles

Five rules for any technical writing.

### 1. Define every term on first use

Curse of knowledge is fatal. Define in context, plain language.

> An _idempotent_ operation produces the same result whether called once or one hundred times. `PUT` is idempotent; `POST` typically is not.

Glossaries help; do not rely on them.

### 2. State preconditions and postconditions

- Pre: "Before you start, the database must be reachable."
- Post: "After this command, the migration history table contains a row for each applied migration."

Skipping pre = reader fails halfway and doesn't know why. Skipping post = reader doesn't know whether to retry.

### 3. Disambiguate every reference

Pronouns are dangerous in technical writing.

- Repeat the noun. "The function returns false" beats "it returns false."
- Use named subjects.
- Avoid demonstratives at paragraph starts. "This migration step …" → "The schema-update step …"

### 4. Show, don't paraphrase

- Code samples are runnable. Missing imports or undefined variables → reader copy-pastes and fails.
- Outputs are real, not invented. Do not write `42` if the system returns `42.0`.
- Pin versions. "On Postgres 15.4, `EXPLAIN (ANALYZE, BUFFERS)` shows: …"

### 5. Lead with the answer

Most technical reading is recovery from a stuck state.

- How-to: steps at the top.
- Reference: signature before explanation.
- Explanation: thesis in first paragraph.
- Reasoning after the answer, not before.

BLUF applies more strictly here than any other genre.

## Instructions and warnings

### Imperative form

"Run the migration." Not "you should run …" and not "the migration should be run."

`should` is permissive; `must` mixes obligation with timing; bare imperative is unambiguous.

### One verb per step

> Stop the service and run the migration.

→

> 1. Stop the service.
> 2. Run the migration.

If the second verb is genuinely a sub-action, lower it visually:

> 1. Stop the service. The container takes 5–8 seconds to drain.
> 2. Run the migration.

### Warnings, cautions, notes

Pick one convention; apply consistently.

- **Warning** — risk of data loss, financial loss, system damage. _Before_ the destructive step.
- **Caution** — risk of incorrect outcome or harder-to-debug state. Before the affected step.
- **Note** — context that improves understanding without changing behavior. After the step.
- **Tip** — optional improvement. After the step.

Warnings inside a step ("Run this — but be careful!") are missed; warnings before the step are not.

### Verification steps

Every procedure ends with a verification.

> Run `kubectl get pods -n production`. All pods report `Running` with the new image tag.

Procedures without verification leave the reader uncertain and inflate support load.

## Document templates by length

### Short reference (≤ 500 words)

Title → one-line summary → signature/syntax → parameters → returns → errors → example → cross-references.

### Medium how-to (500–1,500)

Title (the goal) → one-paragraph summary (when applies / when not) → prerequisites → numbered steps → verification → troubleshooting → related guides.

### Long tutorial (1,500–5,000)

Title (the outcome) → promise → prerequisites → setup → lesson sections (explanation + code + output + what to notice) → result → where to go next.

### Long explanation (1,500–5,000)

Title (the question) → thesis → background/context → argument sections (each ending with a takeaway) → comparison with alternatives → implications → further reading.

## Code samples

Code in docs _is_ docs.

- Syntax-highlight every block.
- Pin language and version where it matters.
- Minimal examples. Trim everything not on point.
- Inline comments only where they teach, not restate the code.
- Realistic identifiers (`customer_id`, `email`; not `x`, `foo`).
- Show output where output is what the reader is checking.
- Test in CI where possible.

## Diagrams, screenshots, tables

Use where prose fails, not as decoration.

- **Diagrams** — relationships among components, flows of data/control. One-sentence caption. Update with the architecture.
- **Screenshots** — UI walkthroughs only; rot when UI changes. Prefer text representation of terminal output.
- **Tables** — comparisons across multiple options/parameters. Two columns is two paragraphs in disguise; tables earn keep at three+.
- **Alt text** — descriptive. "Architecture diagram" isn't alt text; "Web tier sends to API tier; API tier reads/writes Postgres and publishes to Kafka" is.

## Versioning and freshness

Technical content rots. Pages signal currency; writer owns rot.

- Version each doc to software version. "Applies to API v3" / "Applies to versions 2.4–2.7."
- Date each doc. Last-updated prominent.
- Mark deprecation explicitly. Banner: "Deprecated in v3. Use the new API." Keep old content for archival readers.
- Doc-rot calendar — quarterly or per-release sweeps.

## Internationalization

- Short, simple sentences translate well.
- Idioms and metaphors break in translation.
- Use second person consistently; some languages need to know formality.
- Avoid culturally specific examples (Super Bowl, Thanksgiving, IRS) unless audience is US-only.
- Code identifiers stay in original language; translate prose around them.

See `front-end/internationalization.md` for the technical mechanics.

## Common failures

- **Encyclopedic tutorials** — covers four side-quests before the main goal. Cut to goal; link side-quests.
- **Reference written as essay** — long paragraphs where a parameter table would do.
- **How-to written as tutorial** — patiently walks reader through prerequisites they have. Rewrite for the right reader.
- **Conditional sprawl** — branches every two sentences. Split into separate pages or pick most common path.
- **No verification** — reader doesn't know if they succeeded; files a support ticket.
- **Stale screenshots** — UI changed; screenshots didn't.
- **Marketing voice in technical pages** — "Our beautiful, intuitive API empowers …" Technical readers reflexively distrust this.

## Style-guide adoption

Two dominant choices in 2026:

- **Google developer style** — second person, present tense, active, serial comma, conversational-but-not-cute. Permissive on contractions in conversational sections; conservative in reference. <https://developers.google.com/style>
- **Microsoft Writing Style Guide** — second person, present tense, active, "write like you speak." Permissive on contractions throughout. <https://learn.microsoft.com/style-guide>

Pick one, document local extensions, lint against the result. See `front-end/code-style-and-quality.md` § Style guides for tooling.
