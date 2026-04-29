# Sources — 08 Editing and Anti-patterns

## Primary citations

- William Zinsser, _On Writing Well_ (HarperCollins, 30th anniv. ed. 2006). Source for the cut-50%-of-the-first-draft principle, multi-pass editing, and the read-aloud test.
- William Strunk Jr. and E.B. White, _The Elements of Style_ (4th ed., 2000). Source for "Omit needless words" (Rule 17) and the Vigorous Writing principle.
- Roy Peter Clark, _Writing Tools: 50 Essential Strategies for Every Writer_ (Little, Brown, 2006). Source for the disciplined, single-purpose pass approach.
- `front-end/brand-and-copywriting.md` § AI copy DNA — patterns to cut. Cross-reference for the 24-pattern catalog and 14-item Quick checklist; this chapter intentionally does not duplicate.
- McGovern et al., "Stylometric detection of LLM-generated text" (2025). Cited in `front-end/brand-and-copywriting.md`; foundational for sentence-length variance (burstiness) as a structural human-vs-AI signal.

### Prose punctuation anti-patterns

- Kurt Vonnegut, _A Man Without a Country_ (Seven Stories, 2005), "How to Write with Style." Source for the anti-semicolon position ("All they do is show you've been to college"). Backs the rule against semicolons in non-list prose.
- Bryan A. Garner, _Garner's Modern English Usage_ (5th ed., Oxford UP, 2022), entries on semicolons and dashes. Source for the convention that semicolons in narrative prose read as labored, and that em-dashes used as list-wrappers blur sentence-vs-list distinction.
- _Chicago Manual of Style_ (18th ed., 2024), § 6.66–6.67 (em-dashes) and § 6.60 (semicolons). Cited for the canonical use of each punctuation mark, against which the colon-list and em-dash-wrapper patterns deviate.
- Roy Peter Clark, _Writing Tools_ (Little, Brown, 2006), Tool 7 ("Fear not the long sentence"), Tool 8 ("Establish a pattern, then give it a twist"). Backs the position that sentence-pretending-to-be-a-list (the colon-list and em-dash-wrapper forms) is a pattern the reader registers as filler.
- McGovern et al. (cited above) — observed semicolon, colon-list, and em-dash-wrapper frequencies as elevated signals in LLM-generated prose relative to professional human baselines.

## Secondary references

- Stephen King, _On Writing: A Memoir of the Craft_ (Scribner, 2000). Particularly the "second draft = first draft minus 10%" rule.
- Verlyn Klinkenborg, _Several Short Sentences About Writing_ (Vintage, 2012). Source for sentence-level discipline as a primary editing target.
- Constance Hale, _Sin and Syntax_ (Three Rivers Press, rev. ed. 2013). Source for the verb-driven editing approach.
- "On Writing Well" book summary, Shortform: <https://www.shortform.com/summary/on-writing-well-summary-william-zinsser>.

## Curated tool catalog

- Hemingway Editor (hemingwayapp.com) — adverb count, passive voice, sentence-length distribution, reading-grade.
- Vale (vale.sh) — programmable prose linter; can encode style-guide rules and the AI-DNA scan as enforced patterns in CI.
- LanguageTool / Grammarly — grammar and style-pass tools; configure to suppress false positives on technical terms.
- Diff tools (Word's "Track Changes", git diff, Notion-style histories) — make editing passes legible to collaborators and to the writer reviewing their own progress.

## Notes for editors

This chapter is the cross-genre editing counterpart to the genre-specific chapters (04–07). It deliberately points to `front-end/brand-and-copywriting.md` § AI copy DNA for the detailed pattern catalog. When extending, prefer adding genre-specific anti-patterns to the relevant genre chapter; cross-genre patterns belong here.

The five-pass edit is synthesized from Zinsser, Clark, and Klinkenborg. The pass names and order are stable across these sources; the contents of each pass are normalized here for cross-genre application.

Pre-publication checklist is a working tool, not a guideline source — when extending it, add an item only when an editor has missed it on a real artifact and lost time to the miss.
