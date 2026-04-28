# Sources — 08 Editing and Anti-patterns

## Primary citations

- William Zinsser, _On Writing Well_ (HarperCollins, 30th anniv. ed. 2006). Source for the cut-50%-of-the-first-draft principle, multi-pass editing, and the read-aloud test.
- William Strunk Jr. and E.B. White, _The Elements of Style_ (4th ed., 2000). Source for "Omit needless words" (Rule 17) and the Vigorous Writing principle.
- Roy Peter Clark, _Writing Tools: 50 Essential Strategies for Every Writer_ (Little, Brown, 2006). Source for the disciplined, single-purpose pass approach.
- `front-end/brand-and-copywriting.md` § AI copy DNA — patterns to cut. Cross-reference for the 24-pattern catalog and 14-item Quick checklist; this chapter intentionally does not duplicate.
- McGovern et al., "Stylometric detection of LLM-generated text" (2025). Cited in `front-end/brand-and-copywriting.md`; foundational for sentence-length variance (burstiness) as a structural human-vs-AI signal.

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
