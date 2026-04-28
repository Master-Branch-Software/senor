# Editing and Anti-patterns

Drafts are written; pieces are edited into existence. For the 24-pattern AI copy DNA scan, read `front-end/brand-and-copywriting.md` § AI copy DNA. This chapter assumes that scan is part of every editing pass.

## Five-pass edit

One job per pass. Don't do other passes' work in between.

### 1. Truth

- Every claim defensible.
- Every number sourced.
- Every quote attributable.
- Every named fact verified.
- Every example real.
- Hedging matches evidence: "suggests" where suggested, "shows" where shown.

A piece that fails truth cannot be saved by good prose.

### 2. Specificity

- Every abstract noun replaced with a concrete one (or justified).
- Every adverb cut or replaced with a stronger verb.
- Every generic category named.
- Every range narrowed.
- Every "this"/"that" at sentence start checked for unambiguous antecedent.

Signal: > 60% of sentences anchored by a specific quantitative or named element.

### 3. Rhythm

- Read aloud.
- Vary sentence length: 7-word punch among 18-word average; 30-word ramble where the idea earns it.
- Vary paragraph length.
- Replace repeated openers ("This X. This Y. This Z.").
- Break participial-heavy sentences into clauses.

Fastest pass for an experienced editor; most-skipped for an inexperienced one. Skipping produces "technically correct" prose no one wants to read.

### 4. Voice

- Hold draft against brief: register, POV, persona dials.
- Apply "but not" modifiers (direct _but not_ curt; technical _but not_ jargon-heavy).
- Every section has at least one sentence only this writer/persona could have written.

Truth + specificity + rhythm + no voice = competent and forgettable.

### 5. Cut

- Remove 15–25% of words.
- First sentences of paragraphs are highest-value cuts (warm-ups).
- Adverbs first; intensifiers (`very`, `really`, `quite`) second; unearned hedges third.
- Whole sentences that restate the prior in different words.
- Whole paragraphs that don't carry information the next paragraph relies on.

Cut from the longest sentence/paragraph/section first. Fat is roughly proportional.

Five passes in order take roughly the same total time as one undisciplined edit. Visibly different copy.

## When to stop

Three signals another pass is hurting:

1. Sounds like the editor, not the writer.
2. Every sentence is the same length.
3. Shorter than the argument supports.

Stop. Keep the previous version; ship.

## AI DNA scan — cross-genre

Run the 14-item Quick checklist from `front-end/brand-and-copywriting.md` § Quick checklist.

Worth special attention in copywriting work:

- Abstract corporate verbs (`empower`, `leverage`, `streamline`, `unlock`, `drive`, `harness`). DNA pattern 1.
- Rule of Three — every list exactly three items. Pattern 5.
- Uniform sentence length — burstiness is the most reliable structural human-vs-AI signal. Pattern 9.
- Missing contractions where register expects them. Strongest single AI fingerprint.
- Sentence-initial "This" repetition (3+ in a row). Pattern 16.
- "X plays a crucial role in Y" — most formulaic AI construction. Pattern 18.
- "Rather than" hedge comparisons — strongest multi-word AI tell. Pattern 19.
- Em-dash inline-list wrappers (`— A, B, C —`). Pattern 15.

Scan order:

1. `Ctrl+F` for trigger words and phrases.
2. Sentence-length distribution.
3. Contraction count.
4. List-of-three count.
5. Specificity count.
6. Read aloud.

## Genre-specific anti-patterns

| Genre     | Look for                                                                                            | Where                                                        |
| --------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Blogs     | "In conclusion" closers, listicles that don't deliver the count, missing dates, unattributed claims | `blogs-and-articles.md` § Endings; § Listicles               |
| Marketing | Compound offers, manufactured urgency, marketing-speak verbs, generic headlines                     | `websites-and-marketing.md` § What does not work             |
| Web pages | "Empower"/"unleash"/"transform", two primary CTAs, stock-photo visuals                              | `front-end/brand-and-copywriting.md` § Hero section patterns |
| Technical | Conditional sprawl, encyclopedic tutorials, missing verification, stale screenshots                 | `technical-writing.md` § Common failures                     |
| Research  | Buried thesis, unnamed limitations, citation chains, methods that aren't reproducible               | `research-and-academic.md` § Common failures                 |

## Hedging — the difference

### Calibrated (keep)

Honest acknowledgment of uncertainty.

- "These results suggest …"
- "On the available evidence, …"
- "In our sample of 240 …"
- "It is likely that …"

Right voice for science, journalism, any piece where confidence is genuinely partial.

### Filler (cut)

AI-default padding.

- "It is worth noting that …" (hedges the writing act, not the claim).
- "It might be argued that …" (by whom?).
- "Often, in many cases, …"
- "X plays a role in Y" (which role? say it).

If a claim is true, state it.

## Specificity targets

Fastest gain in any editing pass.

- **Abstract nouns** — `solutions`, `stakeholders`, `resources`, `infrastructure`, `engagement`. Replace with the specific thing.
- **Abstract verbs** — `leverage`, `utilize`, `optimize`, `drive`, `facilitate`, `enable`. Replace with the specific action.
- **Generic intensifiers** — `very`, `really`, `quite`, `extremely`. Cut or replace the adjective with a stronger one.
- **Empty adjectives** — `robust`, `seamless`, `holistic`, `cutting-edge`, `world-class`, `industry-leading`. Cut or back with evidence.

Specific replacements are usually longer than abstract originals. Length is earned. Piece becomes shorter overall because specifics replace multi-sentence abstractions.

## Word-count targets

| Surface           | Target                        | Hard cap                             |
| ----------------- | ----------------------------- | ------------------------------------ |
| Headline          | 6–12 words                    | 16                                   |
| Subheading        | 3–10                          | 12                                   |
| Web hero subhead  | 15–25                         | 30                                   |
| Email subject     | 30–50 chars                   | 60                                   |
| Tweet/X           | 100–220 chars                 | 280                                  |
| Meta description  | 150–160 chars                 | 160                                  |
| Research abstract | 200–250 words                 | journal-specific                     |
| Short blog        | 600–1,200                     | —                                    |
| Long-form blog    | 1,500–3,000                   | reader's patience                    |
| White paper       | 3,000–8,000                   | reader's patience                    |
| Tutorial          | 1,500–5,000                   | —                                    |
| README            | 200–800 main + linked details | when subsections need their own page |

Under is fine; over flags a cut.

## Inclusivity

Run after voice, before cut.

- Singular `they` when gender unknown. Avoid "he or she."
- No ableist metaphors (`blind to`, `tone-deaf`, `crazy`, `insane`). Replace with literal description.
- No violent metaphors in product copy (`kill it`, `smash it`, `nail it`) outside sports/military contexts.
- No US-specific references (Super Bowl, Thanksgiving, IRS, "freshman year") unless audience is US-only.
- Use names communities use for themselves; check current preferred terminology.
- Define jargon and acronyms on first use, not in a glossary.

## Fact-checking

1. Names — spelled correctly, role correct, affiliation current.
2. Quotes — verbatim from verifiable source; permission obtained.
3. Numbers — source, date, denominator, methodology.
4. URLs — resolve; archived where rot-likely.
5. Product/company names — spelled and capitalized as the entity does.
6. Claims about systems/features — current as of publication date.
7. Predictions — flagged as opinion, with reasoning visible.
8. Counterarguments — fairly stated, not strawmanned.

Failing fact-checking after publication is harder to recover from than weak prose. If time-constrained, fix this pass before any other.

## Pre-publication checklist

- [ ] Audience brief, register, POV, persona dials confirmed in draft.
- [ ] Five-pass edit complete.
- [ ] AI DNA scan run; every hit addressed.
- [ ] Genre-specific checklist run.
- [ ] Fact-checking complete.
- [ ] Inclusivity pass complete.
- [ ] Read aloud end-to-end.
- [ ] Title, subtitle, lede pass five-second test with one outside reader.
- [ ] CTAs/links work; URLs resolve; alt text present; dates current.
- [ ] If updating: change log noted, last-updated date set, audit trail honest.

Every box clear → publishable. Any unchecked → draft.

## Final principle

Edit for the reader who arrives tired, distracted, on a phone, with twelve other tabs open, after twenty minutes of email. Holding that reader holds anyone.
