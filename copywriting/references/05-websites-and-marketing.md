# Websites and Marketing

Marketing copy spans 280-character ads to 3,000-word landing pages. This chapter covers cross-channel principles and surfaces _not_ covered by `front-end/references/02-brand-and-copywriting.md`. Read front-end/02 first for any web page chrome.

## Where this chapter ends and front-end/02 begins

| Surface                                                      | Primary chapter                                    |
| ------------------------------------------------------------ | -------------------------------------------------- |
| Web pages: hero, pricing, product, about, contact, error     | `front-end/references/02-brand-and-copywriting.md` |
| Microcopy: buttons, forms, empty/error/loading states        | `front-end/references/02-brand-and-copywriting.md` |
| Transactional email: receipts, password resets, trial-ending | `front-end/references/02-brand-and-copywriting.md` |
| SEO copy, structured data, meta descriptions                 | `front-end/references/02-brand-and-copywriting.md` |
| Brand voice, archetypes, voice-of-customer research          | `front-end/references/02-brand-and-copywriting.md` |
| Headline formulas, CTA psychology, social proof              | `front-end/references/02-brand-and-copywriting.md` |
| Marketing email campaigns, newsletters                       | this chapter                                       |
| Ad copy: search, display, social, OOH, print                 | this chapter                                       |
| Scripts: video, podcast, voiceover, IVR                      | this chapter                                       |
| Brochures, packaging, signage                                | this chapter                                       |
| Direct mail, sales sequences                                 | this chapter                                       |

## Cross-channel principles

Five principles that hold everywhere; usually what fails when a campaign underperforms.

1. **One promise per surface.** A landing page that promises three things converts on none.
2. **Specificity beats superlatives.** "Cuts onboarding from 14 days to 3" beats "Industry-leading onboarding speed."
3. **Match the channel's reading mode.** Search ad is read; billboard is glimpsed; podcast script is heard. Same content needs different copy per channel — not the same copy reformatted.
4. **Earn the next click, not the sale.** Most surfaces hand off. Each surface's job is the next step, not the whole job.
5. **Brand voice constant, tone shifts.** Casual brand on a serious topic stays casual in vocabulary; tone shifts to acknowledge gravity. See front-end/02 § Tone ladder.

## Marketing email

Distinct from transactional (front-end/02). Cold outreach to weekly newsletters; copy varies by relationship.

### Subject lines

20–30% of the email's job. Earns the open or doesn't.

- 30–60 characters; mobile clients truncate around 40.
- State the outcome or curiosity, not the sender. "Your invoice is ready" beats "A message from Stripe."
- Avoid all-caps, multiple exclamations, leading emoji on cold lists.
- Personalization: first-name only is detectably automated; reference behavior or shared context.
- A/B test subject lines, not bodies. Cheapest variable, largest delta.

### Preheaders

Extend the subject; do not restate it.

> Subject: "Your April invoice is ready." Preheader: "Due May 1. Auto-pay is off."

### Body

- One CTA per email.
- Lead with the reason the email exists.
- Length: cold outreach 60–100; nurture 150–300; founder-author newsletters 600–2,000. Channel determines length.
- Single column, real text (not images for legibility), one font, alt text on every image.

### Newsletters with a named author

Casual register, first-person singular. Voice is the product.

- One thesis per issue.
- Open with a hook that introduces the thesis, not a meta-greeting.
- Specific anecdotes; named people (with permission); dated references.
- Sign off with a consistent ritual.
- "What I'm reading/watching/building" only if it earns its keep.

## Ad copy

Shorter surface, harder writing.

### Search ads

Google constraints (April 2026): 30-character headlines, 90-character descriptions, three headlines per ad, two descriptions.

- Each headline names a different angle; system rotates and combines.
- Headline 1 — outcome or audience match.
- Headline 2 — proof or differentiator.
- Headline 3 — CTA or urgency (sparingly; platform compliance).
- Description — restate outcome with proof; CTA at end.
- Match copy to search intent, not brand campaign. Mismatched intent is the single biggest cause of low Quality Scores and high CPCs.

### Display ads

Motion-poor, attention-poor environments. Treat as billboards on screen.

- One claim per banner.
- Headline ≤ 6 words.
- Visual carries equal weight; copy is a label.
- CTA = verb-noun pair. Never "Learn more."
- Retargeted ads use different copy than prospecting — recognition is the lever, not introduction.

### Social ads (April 2026 constraints)

- **Meta** — 125-character primary text without truncation; 27-character headline.
- **LinkedIn** — 150-character headline; 600-character primary; assume professional context.
- **X** — 280 characters; mid-scroll reader; lead with the strongest hook.
- **TikTok / Reels** — script first, copy second; spoken hook in first 1.5s is the conversion lever.

Cross-channel best practice: write spoken hook first, visible copy second, CTA third.

### OOH, print, billboards

Reader is moving past, has 1–3 seconds, won't see twice.

- Headline ≤ 7 words.
- One image dominant.
- Brand mark visible at scan distance.
- URL or mnemonic short enough to remember.

> Bad: "Empower your team with a modern workflow platform."
> Good: "Standups are theater. We fix that." [logo]

OOH job is recognition, not conversion. Measure recall.

## Scripts

Spoken copy ≠ written copy. Listener can't scan, skip, or rewind.

### Video

- Hook in first 1.5s. Without it, retention drops below 30%.
- Sentence length 6–14 words; longer becomes unparsable spoken.
- Second person; listener is actor.
- Read aloud while a stopwatch runs. Fit channel optimal length (TikTok 15–60 s; YouTube how-to 4–8 min; webinar 20–45 min).
- Cue visuals: `[CUT TO]`, `[B-ROLL]`. Script is storyboard for the editor.

### Podcast (host-read ads, sponsor reads)

- Conversational; written for the host's natural voice.
- Read in your own voice; rewrite anything that doesn't flow.
- Length: 60s host-read ≈ 150 words; 30s ≈ 75 words.
- Specific CTA with memorable URL or promo code.

### Voiceover

- Punctuate for breath, not grammar. Mark pauses with em dashes or paragraph breaks.
- Avoid sibilance clusters; consecutive `s` clatter on consonant-heavy mics.
- Avoid spoken homophones (`there/their/they're`, `affect/effect`).
- Pronunciation key for proper nouns or unusual words.

### IVR / conversational

- Branch transparently; tell caller what menu they're in before listing options.
- Options ≤ 6 words each; ≤ 5 per menu.
- Confirm input back in caller's words.
- Opt-out to a human at every level.

## Brochures and packaging

Print-only or print-first. No edit after print.

### Brochures

- Cover — one promise, one image, one CTA.
- Inside spread — three to five proof points; 30–80 word blocks; visuals carry equal weight.
- Back cover — CTA, contact, brand mark, URL.
- Walls of text → cut copy, don't shrink type.

### Packaging

- Front face — name + one sentence of category and benefit.
- Side faces — feature/benefit, ingredients/components, regulatory marks.
- Back face — full description, instructions, contact, warnings, country of origin, regulatory text.
- Every claim is legally material. Run regulated claims (medical, food, environmental) past compliance before print.
- Localization changes the artwork; budget from brief stage.

## Direct mail and sales sequences

Multi-touch sequence = one piece of copy across surfaces. Works only if touches are coherent.

- Touch 1 — introduction. Earns the right to send touch 2.
- Touch 2 — proof. Specific outcome, customer, number.
- Touch 3 — offer. Clearest CTA of the sequence.
- Touch 4 — close or soft no. "If now isn't right, here's where to follow our work." Acknowledging non-fit raises trust for future contact.

Email cadence: 2–4 days between cold-outbound; 5–7 days between nurture. Physical direct mail runs weeks apart.

Across the sequence:

- Each touch references the prior one without rehearsing it.
- Each touch is short; cumulative attention is the constraint.
- Each touch stands alone if it's the only one read.

## Cross-channel campaign coherence

Campaign across email, social, paid search, OOH, landing page = one piece of copy in five surfaces. Brief specifies:

- Single promise (one sentence).
- Audience (one paragraph; see `01-foundations.md`).
- Proof (strongest specific number, customer, or fact).
- Visual hook (image/motif that carries across channels).
- CTA (verb-noun pair; one primary, one secondary at most).

Each channel specializes the brief for its reading mode and length budget.

## Testing

- Hypothesis before testing. "Subject-line specificity will lift open rate by ≥ 8%" is testable; "let's see what works" is not.
- One variable at a time.
- Sufficient sample size. 24-hour test on a 5,000-list is noise.
- Measure downstream conversion. Subject lines that win opens lose conversions; chase the user, not the metric.
- Five-second tests with real users beat sophisticated A/B tests when the question is "do they understand the offer."

## What does not work

- Generic headlines describing any competitor in the category.
- Compound offers ("buy now and get 20% off, plus free trial, plus guidebook"). Reader parses none.
- Stock photos. Reader's brain registers "ad" and stops parsing.
- Marketing-speak verbs (`empower`, `unlock`, `leverage`, `drive`, `harness`, `transform`). Default LLM marketing output. See front-end/02 § AI copy DNA for full scan.
- Ambiguous CTAs ("Learn more"). Use unambiguous verb-noun pairs.
- Manufactured urgency. Readers learn to discount the brand.
