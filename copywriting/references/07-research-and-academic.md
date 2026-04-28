# Research and Academic Writing

Research papers, conference articles, dissertations, white papers, lit reviews, technical reports. Audience is small, expert, skeptical. Form is constrained.

## What changes from other genres

1. **Evidence is mandatory.** Every nontrivial claim cited. Reader judges whether sources support what the writer says they do.
2. **Hedging is expected.** "These results suggest" is honest where "this proves" overclaims. Calibrated uncertainty, not the AI-style softening of unsupported claims.
3. **Form is conventional.** Most fields have an expected structure; deviations cost reviewer attention.

## IMRaD

Empirical research in sciences and increasingly social sciences.

| Section      | Question                                     | Length | Voice                                                  |
| ------------ | -------------------------------------------- | ------ | ------------------------------------------------------ |
| Introduction | What is the question and why does it matter? | 10–15% | Active; first-plural acceptable                        |
| Methods      | What did the authors do?                     | 20–30% | Past tense; first-plural or passive                    |
| Results      | What did they find?                          | 20–30% | Past tense; declarative                                |
| Discussion   | What does it mean and what's next?           | 25–35% | Present for claims; past for results being interpreted |

Non-empirical (humanities, math, theoretical CS) replaces IMRaD with field-specific structures: argument-evidence-counter-conclusion (humanities); theorem-proof-corollary (math); problem-related work-approach-evaluation (systems CS). Use the structure expected; do not invent.

## Introduction

Five jobs, one paragraph each, in this order. 600–1,200 words for a standard journal article.

1. Establish the problem. The question this paper addresses.
2. Establish that it matters. Why this question, to whom, with what consequences.
3. Survey related work. What is already known; canonical results; recent challenges.
4. Identify the gap. What existing work hasn't done.
5. Preview the contribution. State explicitly: "This paper contributes …"

Reviewers read intro first. Unclear intro biases the rest of the review.

## Methods

Reviewer-checked most rigorously. Standard: a competent peer could replicate from this section alone.

- Past tense. "We sampled 240 participants from …"
- Active first-plural where the actor is the team. "We administered the questionnaire" beats "the questionnaire was administered." Passive acceptable when actor is irrelevant or when field convention demands (medical methods).
- Specific quantities. "12-item instrument" is concrete; "brief instrument" is not.
- Reproducibility details — software/hardware versions, parameters, random seeds, exact prompts (LLM work), exclusion criteria, sample-size justification.
- Pre-registration linked; deviations identified explicitly.

Subsections (typical): Participants/dataset/corpus; Materials/instruments; Procedure; Analysis plan.

## Results

Reports findings. Does _not_ interpret.

- Lead with the headline result — the one that most directly addresses the question.
- Numbers specific and consistent. Decimal places match across the section; units stated; effect sizes alongside p-values.
- Each table/figure referenced in text; each has a caption that stands alone.
- Confidence intervals AND effect sizes (most fields now expect both, not just p-values).
- Negative results count. Hiding null results is publication bias.

Common failure: interpretation slips in ("this finding suggests …"). Move to discussion.

## Discussion

1. Restate principal findings (one paragraph re-anchor).
2. Interpret against existing literature. Where do these results agree, where disagree, why.
3. Limitations honestly. Sample size, measurement, scope, alternative explanations. Reviewers respect explicit; punish hidden.
4. Implications — practice, theory, policy.
5. Future work. What this paper opens up; the next experiment.

Hedging language appropriate throughout: "these results are consistent with," "one possible explanation is." Calibrated, not weakness.

## Abstract

Most readers see the abstract and nothing else. 200–300 words; structured form: Background, Methods, Results, Conclusions. Some journals add Objectives or Limitations.

- Background (1–2 sentences): what's known, what gap motivates the study.
- Methods (2–4): design, population, key variables, analysis.
- Results (2–4): principal findings with key quantities.
- Conclusions (1–2): take-home and most important implication.

Constraints:

- Strict word-count compliance. Reviewers reject over-length abstracts before reading the body.
- No citations (most journals).
- No abbreviations on first use unless well-known in field.
- Coherent standalone — reader has not seen body.

## Hedging — academic kind

Academic hedging is honesty about uncertainty. It is the right response to evidence that does not support a definitive claim.

### Useful hedge verbs

`suggests`, `indicates`, `is consistent with`, `may`, `could`, `appears`, `is associated with`, `tends to`. Calibrated to the strength of the evidence.

### Verbs to avoid as hedges (they overclaim)

`proves`, `shows definitively`, `demonstrates that`. Empirical work is _consistent with_ a hypothesis, not _proof of_ it. Reserve these for cases where the evidence is overwhelming.

### Verbs to avoid as filler hedges (they read as AI-default)

- "It is worth noting that …" (hedges the writing act, not the claim).
- "It might be argued that …" (by whom?).
- "Often, in many cases, …"
- "X plays a role in Y" (causes? influences? correlates? say which).

If a claim is true, state it. If not, fix the claim. See `08-editing-and-anti-patterns.md` § Hedging — the difference for the editing-pass cut/keep test.

## Active vs passive in academia

Major journals (_Nature_, _Science_, APA 7th, most science-writing handbooks) prefer active with first-plural where the writer is the actor.

- Active and first-plural in introductions, when stating contributions, and when describing actions taken.
- Passive when actor is genuinely unknown or irrelevant ("the samples were stored at –20 °C"), and where field convention strongly prefers it.
- Mix purposefully, not by accident.

## Citations

Cite every nontrivial claim — anything specific enough that a reasonable reader could disagree without evidence.

- Cite the original, not a survey citing the original.
- Cite numbers exactly. "Smith et al. (2023, p. 47) report a 12.4% reduction."
- Cite hostile sources, not just friendly ones.
- Quote sparingly. Paraphrase and cite.

### Citation styles

| Style              | Field                                            | Format                              |
| ------------------ | ------------------------------------------------ | ----------------------------------- |
| APA                | Psychology, education, social sciences, business | Author-date in text; reference list |
| MLA                | Humanities, esp. literature/languages            | Author-page in text; works-cited    |
| Chicago / Turabian | History, some humanities, general books          | Notes-bibliography or author-date   |
| IEEE               | Engineering, CS                                  | Bracketed numbers; numbered list    |
| Vancouver          | Medicine, life sciences                          | Superscript numbers; numbered list  |
| Harvard            | UK/Australia social sciences                     | Author-date; reference list         |

Field/journal dictates. Use a reference manager (Zotero, Mendeley, BibTeX). Manual formatting is a source of errors reviewers notice.

## Tables and figures

- Self-contained. Caption + figure understood without body text.
- Each referenced in text.
- Caption above tables, below figures (most styles).

Anti-patterns:

- Pie charts when bar charts would do.
- 3D bar charts.
- Color-only encoding.
- Tables with merged cells (don't survive screen readers).

## Plain-language summaries

Funder/journal requirement. Translates abstract into language a literate non-specialist can read.

- 100–250 words.
- No jargon; define every domain term.
- No statistical terminology — "the result was unlikely to be due to chance" instead of "p < 0.01."
- Opens with the question the public would ask.

A plain-language summary is its own writing task, not an abridgement.

## Common failures

- **Buried thesis.** Contribution in second paragraph of discussion, not in abstract or intro.
- **Reviewer-bait hedging.** "This study has limitations" without naming any.
- **Citation chains.** "Smith (2020) cites Jones (2010), who found …" Cite Jones directly.
- **Bloated related work.** Literature dump rather than argument. Cluster citations by claim.
- **Marketing voice creep.** White papers and corporate research drift from analysis to advocacy.
- **Inflated quantitative claims.** "Significant" used as synonym for "important."
- **Methods that aren't reproducible.** Reviewers reject this more than any other issue.

## White papers and industry research

Hybrid: research conventions + marketing purpose.

- Research first, marketing second. Credibility carries the marketing.
- Cite real sources; do not fabricate proprietary data without disclosure.
- Disclose conflicts upfront ("This paper was sponsored by …").
- Brand voice in executive summary; research voice in body. Two different registers (see `02-register-and-formality.md`).
- Keep argument structure even when the conclusion favors the sponsor.
