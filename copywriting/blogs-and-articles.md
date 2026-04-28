# Blogs and Articles

Long-form online prose. This chapter is about the prose. For blog _page_ chrome (TOC, byline, dates, structured data), see `front-end/brand-and-copywriting.md` § Blog or article page.

## Genres at a glance

| Genre                     | Length       | POV                   | Register            | Structure                                        |
| ------------------------- | ------------ | --------------------- | ------------------- | ------------------------------------------------ |
| News / explainer          | 400–1,200    | Third                 | Consultative        | Inverted pyramid                                 |
| How-to / listicle         | 800–2,000    | Second                | Consultative–casual | Promise → headed steps → recap                   |
| Personal essay            | 1,200–4,000  | First singular        | Casual–intimate     | Scene → reflection → scene; thesis emerges       |
| Op-ed / argument          | 700–1,200    | First singular/plural | Consultative–casual | Claim → evidence → counter → rebuttal → restate  |
| Long-form feature         | 3,000–10,000 | Third often           | Consultative        | Lead anecdote → context → reporting → resolution |
| Newsletter (named author) | 600–2,000    | First singular        | Casual              | One thesis per issue; serialized voice           |
| Technical post            | 1,000–3,000  | First plural / second | Consultative        | Problem → setup → demo → trade-offs              |
| Review / criticism        | 600–2,000    | First singular        | Consultative–casual | Subject framing → analysis → verdict             |

Genre dictates structure. Personal essay as inverted pyramid is dead on arrival; explainer as personal essay loses every reader who came for the answer.

## The lede

First sentence and paragraph carry the most weight. Three approaches:

1. **BLUF.** State the point. News, briefings, technical posts, business writing.

   > GitHub Actions deprecated `set-output` on October 11, 2024. Workflows that still use it stop working on January 1, 2025. Three replacements exist; this post recommends one.

2. **Anecdotal.** A specific scene that leads to thesis by paragraph 2–3. Features, essays, magazine.

   > By the time the page-load alert fired at 2:47 a.m., the on-call engineer had already been awake for thirty minutes — not because of the alert, but because his neighbor's dog wouldn't stop. Coincidences pile up, and they don't always mean anything. This one did.

3. **Provocative claim.** Op-eds and arguments.

   > Most standups are theater. Replace them with async updates and a ten-minute weekly sync.

Whichever lede: must be specific. "In today's fast-paced world, communication is more important than ever" is grounds for not publishing.

## Inverted pyramid

For news, explainers, reference-leaning blogs. Most newsworthy first; supporting detail descending. Web reading is scanning; the inverted pyramid is the structural response (Nielsen Norman).

1. Lede — the answer or central event.
2. Nut graf — why this matters now and to whom.
3. Body — supporting detail, quotes, mechanism, evidence.
4. Background — context for careful readers.
5. Tail — related items, links, further reading.

A reader who quits at any level still has a coherent story.

## Narrative arc

For features, essays, personal pieces. Inverted pyramid spoils the journey.

1. Hook — scene/image/moment.
2. Stakes — what is at risk.
3. Tension — obstacle, unknown, disagreement.
4. Reporting/argument — meat.
5. Turn — view shifts or evidence resolves.
6. Resolution — what we now know.
7. Coda — return to opening, transformed (optional but powerful).

## Subheads

Scannable contracts. A reader who reads only the H2s should know what the piece argues.

- Full clauses, not category labels. "Why standups fail" beats "Failures."
- Parallel within a piece. If one is "Why X fails," the next is "Why Y also fails," not "The benefits of Z."
- One thought per subhead. Two-clause subheads signal an unsplit section.
- Frequency: every 200–400 words online; longer in academic/print.

## TL;DR and excerpt

Pieces over 1,200 words benefit from a TL;DR — three to five sentences with the thesis and major moves. Standalone abstract for the time-pressed reader, not a chronological summary. Not a substitute for a strong lede.

Meta description (SEO excerpt) is separate — 150–160 characters, written for the search results page. See `front-end/brand-and-copywriting.md` § SEO.

## Headlines

Three jobs:

1. Tells the truth about the piece.
2. Names a specific outcome or claim.
3. Earns the click without dishonest curiosity gaps or manufactured urgency.

Working formulas (deeper catalog in `front-end/brand-and-copywriting.md` § Headline formulas):

- Specific outcome — "How to reduce INP by 40% on a Next.js app."
- Counter-conventional — "Stop A/B-testing your homepage."
- Named subject — "What three months of editor logs taught me about indecision."
- Numbered list (only if the piece is a list) — "Seven blog formats that still work in 2026."

Five-second test: stranger reads headline, says one sentence about the piece. If close, it works.

## Pull quotes and emphasis

For 1,500+ words, one or two pull quotes break up text and surface strongest sentences.

- Pull quote = most representative, not most surprising. Tells the scrolling reader what the piece is about.
- Set off visually — separate weight, color, or left rule.
- Don't pull-quote another source's quote unless it's the heart of the piece.

Italics/bold are punctuation, not decoration:

- Italics — titles, foreign terms on first use, occasional emphasis where word stress matters.
- Bold — terms being defined, key phrases in scannable lists. Never general emphasis.
- A paragraph with three bolded phrases reads as written by someone who doesn't trust their sentences.

## Pacing

Long-form is judged at the paragraph level. Three controls:

1. Paragraph length variation. 3–5 sentences baseline; one-sentence paragraphs for emphasis.
2. Sentence length variation. See `foundations.md` § Sentence rhythm.
3. Section length variation. Some H2s 200 words; one or two 600. Reader needs both breathing room and depth.

A 3,000-word piece with twelve identical 250-word sections reads as content-mill product.

## Attribution

Online readers default to skepticism. Earn trust by attributing every nontrivial claim:

- Inline links for online sources.
- Block quotes with attribution underneath.
- Numbers cited with denominator and source ("3.2% of users, n = 12,400, internal data Q1 2026").
- Personal experience flagged ("In my own experience, …").
- Counterarguments named, not hidden.

## Updates and dates

Time-sensitive posts: publication date AND last-updated date prominently.

When updating an old post:

- Update last-updated date.
- Brief note at top: what changed, when.
- Strike through removed content if the original was wrong (keeps audit trail).
- Live-blog posts timestamp each addition.

## Listicles

Earn the number. "Seven things" must deliver seven distinct, parallel things.

- Same type, same scope, same level of specificity.
- Each item: own H2 or numbered subhead, one-sentence promise, 100–300 words body.
- Ordered numbering only when the order is meaningful (priority, chronology, escalation).
- Even a listicle has a thesis.

## Internal links and outbound

- Link to your own piece on a sub-topic when the reader might follow it; don't pile in five per paragraph.
- Link out to original sources, especially for data, definitions, specific claims.
- Anchor text describes destination. No "click here."

## Endings

Strong:

- **Coda** — return to opening with new meaning.
- **Implication** — what reader should do, change, or watch for.
- **Open question** — name the thing the piece couldn't resolve.
- **Synthesis** — thesis restated, sharper than at the top.

Avoid:

- "In conclusion, …" — meta-commentary; AI-default opener.
- "Hopefully this post has helped you …" — undermines confidence.
- "What do you think? Let me know in the comments." — empty engagement bait.
- "Thanks for reading!" — fine for ritualized newsletters; filler for one-offs.
