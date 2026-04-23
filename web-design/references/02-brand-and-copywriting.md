# Brand and Copywriting

Copy is the first design material users consume. Bad copy sinks beautiful visuals; sharp copy rescues plain ones. A consistent voice turns a collection of pages into a brand.

## Decide the brand first

Before writing a single sentence of public-facing copy, produce a short document that answers:

- Who the brand serves (one sentence, specific).
- What the brand offers (one sentence, the category and the differentiator).
- Why the brand exists (one sentence, grounded in a user problem).
- The brand personality in three to five adjectives.
- The brand archetype (see below).
- The core voice attributes (three to five, each paired with "but not" modifiers).
- A short list of words and phrases the brand uses, and a matching list it avoids.

Pin this document somewhere every writer and designer can see it. Every piece of copy is evaluated against it.

Worked example of a brand sheet for a fictional error-tracking SaaS called Flint:

```md path=null start=null
Audience: senior backend engineers responsible for production observability at Series B–D startups.
Offer: error tracking that groups by root cause, not by stack frame.
Reason to exist: teams drown in duplicate alerts because tools group the symptom, not the cause.
Personality: precise, low-drama, senior.
Archetype: Sage (primary), Hero (secondary).
Voice:
- Direct, but not curt.
- Technical, but not jargon-heavy.
- Wry, but not clever for its own sake.
- Senior, but not condescending.
Use: root cause, signal, noise, on-call, ship, incident, postmortem, paging.
Avoid: unleash, empower, revolutionize, AI-powered, ninja, rockstar, game-changer, synergy.
```

## The 12 Jungian brand archetypes

A framework for emotional positioning. Pick one primary archetype; a secondary archetype is permitted. Mixing three or more produces incoherence.

Stability and control

- Caregiver — nurturing, protective, compassionate. Johnson & Johnson, Dove.
- Ruler — authoritative, structured, exclusive. Rolex, Mercedes-Benz.
- Creator — imaginative, expressive, original. Lego, Adobe.

Learning and freedom

- Innocent — optimistic, honest, simple. Coca-Cola, Dove (shared).
- Sage — wise, analytical, objective. Google, The Economist.
- Explorer — independent, adventurous, pioneering. Patagonia, Jeep.

Belonging and enjoyment

- Jester — playful, irreverent, humorous. Old Spice, M&M's.
- Lover — passionate, committed, intimate. Chanel, Haagen-Dazs.
- Everyman — relatable, friendly, down-to-earth. IKEA, Target.

Mastery and risk

- Hero — courageous, bold, triumphant. Nike, FedEx.
- Outlaw — rebellious, disruptive, raw. Harley-Davidson, Virgin.
- Magician — transformative, visionary, inspired. Apple, Disney, Tesla.

Each archetype implies a color temperature, a typographic register, a motion vocabulary, and a copy voice. A Sage brand writing in Jester voice reads as untrustworthy.

## Voice and tone

Voice is the brand's consistent personality. Tone is the situational expression of that voice. Mailchimp's distinction is the clearest model: voice is fixed, tone adapts.

Define voice with three to five attributes, each with a "but not" qualifier, for example:

- Plainspoken, but not dumbed down.
- Genuine, but not overly sincere.
- Dry humor, but not snarky.
- Translator, but not condescending.

### Tone ladder

Voice is constant. Tone shifts along four dials — formality, warmth, urgency, playfulness — per context. Document the expected position of each dial for every surface.

- Marketing hero — mid formality, warm, low urgency, brand-appropriate play.
- Onboarding — low formality, warm, low urgency, playful permitted.
- Empty state — low formality, warm, low urgency, gently playful.
- Success and celebration — low formality, warm, low urgency, brief play permitted.
- Notification or toast — low formality, neutral warmth, low urgency, no play.
- Error caused by user input — mid formality, warm, low urgency, no play; never blaming.
- Error caused by system — mid formality, warm, mid urgency, no play; acknowledge explicitly.
- Destructive confirmation — high formality, neutral warmth, high urgency, no play.
- Billing and payment — high formality, neutral warmth, low urgency, no play.
- Security and privacy — high formality, neutral warmth, mid urgency, no play.
- Legal — high formality, neutral warmth, low urgency, no play.
- Churn or cancellation — mid formality, warm, low urgency, no play; never guilt.
- Outage or incident — high formality, warm, high urgency, no play; name the impact.

A Sage brand in an outage keeps the voice (precise, measured) and shifts the tone (high urgency, direct acknowledgement). A Jester brand holds the playfulness on an outage page only long enough to acknowledge the problem; the primary information is straight.

## Voice-of-customer research

Professional copy is harvested, not invented. The fastest way to reach a brand-native voice is to collect what real users already say and cluster it.

### Sources, ranked by signal

- Customer interviews — 8–12 conversations with recent signups, active users, and recent churners. Record with permission. Transcribe verbatim.
- Sales-call recordings — Gong, Chorus, or manual notes. Objection language is gold for headlines; reassurance language is gold for FAQs.
- Support tickets and chat logs — cluster by root cause to find the pains users actually name. The phrases users type to describe a broken thing are the phrases that should appear in feature copy.
- Review platforms — G2, Capterra, App Store, Amazon, Trustpilot, Product Hunt. Five-star reviews surface benefit language; three-star reviews surface honest limits; one-star reviews surface the sharpest pain.
- Community channels — subreddits, Discord, Slack, X search, LinkedIn comments on competitor posts. Filter for named audience terms and competitor names.
- Win/loss interviews — won deals reveal the deciding reason (headline candidate); lost deals reveal the missing proof (objection-handling copy).
- Search queries — Google Search Console queries that land on the site, plus `People also ask`, `Related searches`, and high-volume long-tail questions from Ahrefs or Semrush.

### Harvesting method

1. Pull 150–300 raw quotes across all sources for one product or audience.
2. Strip identifying information; keep the quote verbatim.
3. Tag each quote by job-to-be-done, emotion, and funnel stage (awareness, consideration, decision, activation, retention, churn).
4. Cluster tagged quotes. Dominant clusters become sections. The most specific quote in a cluster becomes the testimonial for that section.
5. Extract an audience vocabulary — the 20–50 recurring nouns and verbs real users reach for. Copy must use those words; marketing-native synonyms get cut.

### Turning raw language into copy

- Keep the user's nouns; replace any internal terminology. If users say "alerts", the product does not "notify" them.
- Keep the user's verbs where they are concrete. "Drowning" beats "overwhelmed"; "ship" beats "release to production".
- Strip qualifiers ("sort of", "kind of", "I think") unless the brand voice is deliberately hedged.
- Never paraphrase a testimonial into brand voice; it becomes generic and stops functioning as evidence.

Bad (copy written inside a conference room):

> Empower developers to unify their observability stack and accelerate incident resolution.

Good (copy written from 30 customer interviews):

> Group errors by root cause, not by stack frame. Stop paging the same engineer for the same bug five times a day.

## Copywriting frameworks

Frameworks are scaffolds, not templates. They speed up drafting; they never replace editing.

### AIDA

Attention, Interest, Desire, Action. Legacy advertising framework, still useful for landing-page hero sections.

- Attention — a specific claim that names the audience and the benefit.
- Interest — evidence that the claim is true (a number, a named customer, a mechanism).
- Desire — one crisp sentence about the outcome, framed in the user's words.
- Action — a single primary CTA with a verb and a specific noun.

Worked example — time-tracking SaaS for billable-hour agencies:

- Attention: "Timesheets in four minutes a week, not forty."
- Interest: "Clients bill by the hour. Teams lose Friday afternoons reconstructing them. Flint fills the timesheet from the calendar and commits."
- Desire: "No timers, no browser extensions. Review a draft, approve, send."
- Action: "Try it on this week's timesheet."

### PAS

Problem, Agitate, Solution. Works well for sales pages where the problem is tangible.

- Problem — name the painful situation in the reader's words.
- Agitate — describe the consequences specifically and honestly.
- Solution — present the product as the direct counter, with proof.

Worked example — dental-booking software:

- Problem: "Front-desk staff spend 22% of the day on the phone rescheduling no-shows."
- Agitate: "That is four to six hours of trained staff time a week, and every no-show is a slot another patient would have booked."
- Solution: "Automated reminders, two-way SMS confirmation, and an instant-rebook link. Surgeries using Denta fill 94% of their cancellations within two hours."

### BAB

Before, After, Bridge. A softer sibling of PAS, good for lifestyle and SaaS.

- Before — the current reality.
- After — the improved reality, described sensorily.
- Bridge — the product as the route from one to the other.

Worked example — incident-management tool:

- Before: "Every release ends with a 2 a.m. war-room ping."
- After: "The on-call engineer sees a single root-cause alert and goes back to bed."
- Bridge: "Flint groups by root cause, not stack frame."

### 4 Ps

Promise, Picture, Proof, Push. Effective for ad copy and product tiles where brevity is forced.

### FAB

Features, Advantages, Benefits. A reminder that features are inputs, advantages are comparisons, benefits are what the user actually cares about. Never lead with features.

Worked conversion:

- Feature: "OKLCH color palette generator."
- Advantage: "Perceptually uniform; colors stay balanced at any lightness."
- Benefit: "Dark mode looks right the first time, without hand-tuning."

### StoryBrand (Donald Miller)

The customer is the hero. The brand is the guide. The product is the plan. Applies to nearly every consumer and B2B site.

A StoryBrand hero section follows this pattern:

1. The hero has a problem (stated in user words).
2. They meet a guide (the brand, with evidence of empathy and authority).
3. The guide gives a plan (three simple steps).
4. The guide calls them to action (one primary CTA, one secondary).
5. That ends in success (the stakes of winning).
6. …and helps them avoid failure (the stakes of losing, implicit).

### PASTOR, ACCA, 4U

Additional frameworks for specific contexts (direct response, ad copy, headlines). Pick one, apply it, edit ruthlessly; avoid framework-hopping mid-page.

## Headline formulas

A headline passes the five-second test only when it names a specific outcome for a specific audience. These formulas consistently do that work.

- "How to [outcome] without [sacrifice]." — "How to deploy on Friday without breaking production."
- "[Outcome] for [specific audience]." — "Invoicing for freelance designers who charge in three currencies."
- "The [number] [thing] that [outcome]." — "The four queries that explain 80% of slow page loads."
- "[Question the user is asking]." — "Why does the CSS cascade keep breaking?"
- "[Time or effort] to [outcome]." — "Two minutes from signup to first deploy."
- "[Specific promise], [specific proof]." — "Ship 3× faster. 300+ engineering teams do."
- "Stop [current painful behavior]. Start [better behavior]." — "Stop grepping logs. Start reading traces."
- "[Category] without the [usual drawback]." — "Salesforce without the consultants."
- Ogilvy fact-first — "At 60 miles an hour the loudest noise in this new Rolls-Royce comes from the electric clock." Works when the product has a single startling, verifiable fact.
- Listicle — "Seven things competitors do on the homepage that this one does not." Earn the specificity before using it.

Headlines that almost always fail:

- Abstract verbs — "unleash", "empower", "transform", "revolutionize".
- Category claims without proof — "The leading…", "The #1…".
- Two promises joined by "and" — the reader gets neither.
- Anything that could describe any competitor in the category.

Bad: "The leading platform for modern teams."
Good: "Schedule 1:1s across four time zones without a spreadsheet."

## Hero section patterns

A hero must answer, within five seconds, what this is, who it serves, and what to do next. Canonical structure:

1. Headline — specific outcome or proposition, not a slogan. Around 6–12 words.
2. Subheadline — one sentence of proof or concrete detail. Around 15–25 words.
3. Primary CTA — a verb plus a specific noun ("Start a free trial", "See pricing", not "Learn more").
4. Optional secondary CTA — lower-commitment action (demo, docs, contact).
5. Evidence — a logo bar, a headline number, a named testimonial, or a screenshot that proves the claim.
6. Visual — a screenshot of the product, not a stock image of a diverse team staring at a laptop.

Avoid:

- "The leading…" or "The #1…" without a source.
- Vague verbs such as "empower," "unleash," "transform."
- Two primary CTAs fighting for attention.
- A headline that could describe any competitor in the category.

Bad hero:

> Headline: Empower your workflow.
> Subheadline: An AI-powered platform that revolutionizes how modern teams get work done.
> CTA: Learn more

Good hero:

> Headline: Group errors by root cause, not stack frame.
> Subheadline: Flint clusters duplicate alerts so on-call engineers see one incident, not one hundred. Used by 400+ engineering teams at Stripe, Linear, and PlanetScale.
> Primary CTA: Start tracking free
> Secondary CTA: See a live demo

## Value proposition

One sentence, on the home page, that states:

- the end-benefit offered,
- to whom,
- against what alternative,
- with what unique mechanism.

If any of those four elements is missing, the proposition is generic.

Bad: "A modern invoicing platform for modern businesses."
Good: "Freelance designers bill clients in multiple currencies without losing 3% to FX, because Ledger settles in the currency the client pays in."

## Page-type copy patterns

Different pages do different work. Each common page type has its own copy skeleton.

### Home page

Sections, top to bottom:

- Hero (as above).
- Trust strip — logo bar or a single pull-number ("Trusted by 400+ engineering teams"). Real logos with written permission only.
- Problem framing — one sentence of the painful status quo (PAS-style).
- Solution — three to five blocks, each with an outcome headline, a one-sentence description, a screenshot, and a supporting micro-testimonial.
- Social proof — a named case study with a concrete metric.
- Objection handling — the top three reasons buyers hesitate, answered in short FAQ form.
- Secondary CTA block — for visitors not ready to buy (book a demo, read the docs, subscribe).
- Footer.

Home-page copy rules:

- The home page is a map, not an essay. Every section has a single sentence a visitor can remember.
- The product appears visually above the fold. Without that, the visitor does not know what the product is.
- Never rely on a video or hero animation to communicate the value proposition; the headline must carry it alone.

### Pricing page

Structure:

- Headline that frames pricing as a decision, not a rate card. "Pay for the engineers on call, not the total headcount" beats "Pricing that scales with your team".
- Billing toggle if annual and monthly both exist. Default to annual; show the saved amount.
- Three to four tiers maximum. Most buyers cannot compare more than four options at once.
- Tier names signal who each tier is for, not prestige. "Solo", "Team", "Enterprise" beats "Bronze", "Silver", "Gold".
- Anchor the middle or most-recommended tier visually with a badge and a slightly stronger outline.
- Per-tier structure: name, one-sentence positioning, price, primary CTA, a short "best for" line, then the feature list.
- Feature list pattern: each tier after the first starts with "Everything in [previous tier], plus:". Prevents copy duplication and makes upgrade value legible.
- Enterprise tier has no price, a "Contact sales" CTA, and lists differentiators that matter at scale: SSO, RBAC, audit log, VPC peering, procurement-friendly invoicing, SLA, dedicated support.
- Below the tiers: a concrete comparison of features buyers actually compare (usage limits, included seats, SSO, audit log). A long feature matrix beats marketing pictograms.
- Objection-handling FAQ — "What happens when limits are exceeded?", "Can plans be cancelled any time?", "Is there a free trial?", "Is there an annual discount?", "What counts as a seat?".
- Money-back or cancellation language in plain words.

Pricing copy rules:

- Never hide the price. If enterprise is custom, say so in one sentence.
- Never compare against competitors by name on the primary pricing page; put that on a dedicated comparison page.
- Never use dark patterns — pre-checked upgrades, hidden add-ons, cancellation mazes.
- Numbers are specific. "Up to 10,000 events per month" beats "Generous limits".

### Product or feature page

Structure:

- Job-to-be-done headline that names the outcome, not the feature.
- One-sentence subhead that names the mechanism.
- Product screenshot, annotated if the interface needs explaining.
- Three to six benefit sections, each with: outcome headline → one-sentence explanation → supporting visual → proof (quote or number).
- Comparison to alternatives, if the feature replaces an existing workflow. Prefer comparison to the reader's current painful behavior, not to a named competitor.
- Named customer quote on the feature specifically.
- Secondary CTA — docs, related feature, or a request-for-access link.

Feature-page copy rules:

- Every benefit headline names the outcome the user cares about, not the technical mechanism. "Stop copying SSH keys between machines" beats "Certificate-based authentication".
- A feature without a named user quote is a feature that does not yet have a proven user.

### About page

An About page is the fastest-read page on most sites and the most mis-written.

Structure, in this order:

- One-sentence mission, user-facing, not internally-facing. "Help on-call engineers sleep through the night" beats "Pioneering the next generation of developer tools".
- Origin paragraph — the specific moment the problem became unbearable. Two to four sentences, first-person if the founder is public.
- What the company does, in one paragraph, plain language.
- Proof — customers, funding if it matters to the audience, named press.
- Team — real photos, real names, real titles. Small teams list everyone; larger teams list leadership.
- Values — three to five, each with a one-sentence explanation of how it shows up in the product. Skip values that are table stakes (customer obsession, integrity).
- Hiring block with a direct link to open roles.

About-page copy rules:

- Founder voice is permitted here more than anywhere else on the site.
- No stock photos of smiling teams in front of laptops.
- The team page uses the same voice as the rest of the site; it is not a LinkedIn dump.

### Blog or article page

Structure:

- Title — question-aligned, specific, benefit-bearing. "How to reduce INP by 40% on a Next.js app" beats "INP tips".
- Deck or subtitle — one sentence, 15–30 words, stating the promise of the piece.
- Author byline with face, role, and a link to the profile.
- Published and updated dates.
- Read-time estimate.
- Table of contents for posts longer than 1,200 words.
- TL;DR block for posts over 1,500 words: three to five sentences of the thesis.
- Body with descriptive subheads — every H2 a scannable question or promise.
- Pull quotes for key claims.
- Inline images with descriptive alt text and figure captions.
- Further reading with three to five links.
- Short author bio at the foot with a subscribe or follow CTA.

Blog copy rules:

- No listicle headlines without the actual list ("7 things…" must deliver seven things).
- No date-free posts on evergreen topics; dates are required for time-sensitive ones.
- The first 100 words deliver the core promise; readers bounce if the intro is a warm-up.

### Contact and sales pages

- Segment the form at the top: "Sales", "Support", "Press", "Careers". Route each to the right team.
- Sales form asks for company, team size, and primary use case — not fifteen fields.
- Support points to documentation, status page, and community first; a contact form third.
- Every form confirms submission and states when the sender will hear back.

### Error pages

404:

- Acknowledge the situation in one sentence, no blame.
- Offer the three most likely next destinations: home, search, recent pages.
- Never make a joke at the cost of utility; if the joke works for the brand, put it after the useful part.

500:

- Acknowledge the system's failure explicitly.
- Give the user one action (retry, go home).
- Provide a status-page link if one exists.
- Never expose a stack trace.

Bad 404:

> Oops! Something went wrong.

Good 404:

> That page does not exist. It may have been moved. Try the docs, the blog, or the home page.

### Legal and footer copy

- Terms of service and privacy policy — plain-language summary at the top, full legal body below. Include "Last updated" dates.
- Cookie policy — what cookies are set, why, and how to change preferences.
- Accessibility statement — conformance target (WCAG 2.2 AA), known limitations, contact for reports.
- Security page — vulnerability-disclosure policy, bug-bounty program if one exists, contact address.
- Footer links use noun phrases, not marketing headlines. "Pricing" beats "Find a plan that fits".

### Email and transactional copy

Transactional email is often the highest-read text a product ships. Treat it as carefully as the home page.

Subject lines:

- State the outcome or action, not the sender. "Your April invoice is ready" beats "A message from Flint".
- Keep to 30–50 characters where possible; mobile clients truncate around 40.
- Avoid spam-triggering patterns (all caps, multiple exclamation marks, leading emoji).

Preheaders:

- Extend the subject, do not repeat it. "Due May 1. Auto-charge is off." beats "Your invoice is ready."

Body:

- One action per email. Multiple CTAs reduce clicks on the primary one.
- Lead with the reason the email was sent. "A password reset was requested for this address." then the button, then the expiry.
- Signed from a named human for relationship-sensitive emails; from the brand for system notifications.

Canonical transactional emails every product ships:

- Welcome — one CTA, the smallest next step, not a tour.
- Email verification — expiry, alternate link.
- Password reset — expiry, "Ignore this email if no reset was requested" line.
- Login from a new device — device, location, and a "Was this not you?" link.
- Receipt or invoice — line items, total, VAT, payment method last 4.
- Trial ending — days left, what happens on expiry, the upgrade link.
- Failed payment — what failed, what to do next, the retry link.
- Cancellation — confirm, state what ends and when, offer to resubscribe.
- Export or data deletion — link, expiry, support contact.

## Microcopy

Small strings with outsized impact. Nielsen Norman Group's 3 C's — clarity, concision, character — and 3 I's — inform, influence, interact — are the working model.

### Buttons

- Use verbs. "Save changes", not "OK". "Delete account", not "Confirm".
- Match the verb to the user's intent, not the system's action.
- Disabled states must still explain why ("Fill in your email to continue").

Bad: "OK" / "Submit" / "Learn more"
Good: "Save changes" / "Send invoice" / "See pricing"

### Forms

- Every `<input>` has a visible `<label>`. Placeholders are not labels.
- Required fields mark required, not optional, unless most fields are optional.
- Error messages identify the field, state the rule, and suggest a fix.
- Success messages confirm what changed.
- Validate on blur, not on every keystroke, unless there is a strong reason.

Bad error: "Invalid input."
Good error: "Password needs at least 10 characters, including one number."

### Empty states

- Name the reason the state is empty.
- Give the user the single action that populates it.
- Avoid cute illustrations without purpose. A blank page with one clear button beats a whimsical drawing without direction.

Bad: "No results."
Good: "No projects yet. Start one to invite collaborators." [Create project]

### Error states

- Never blame the user.
- Tell them what happened, why, and what to do next.
- Offer a retry, a rollback, or a way to contact support.
- Preserve their input.

Bad: "An error has occurred."
Good: "Flint couldn't save the draft because the connection dropped. The text is still here — try saving again."

### Loading states

- Under 400 ms: no indicator.
- 400 ms to 1 s: a spinner or subtle pulse.
- Over 1 s: a skeleton screen that approximates the final layout, preventing layout shift on content arrival.
- Over 10 s: an explicit progress indicator, ideally with an estimated time.

### Confirmations

- Echo back what the user did in their own language.
- Suggest the next sensible action.

Bad: "Done."
Good: "Invoice sent to acme@example.com. See sent invoices."

## CTA psychology

Verbs are the floor, not the ceiling. Strong CTAs also carry benefit, specificity, and friction reducers.

- Name the outcome, not the action. "Start tracking errors" beats "Sign up".
- Reduce friction below the button. "No credit card required. Cancel in one click." kills three objections in nine words.
- Keep the primary CTA unique per page. Secondary CTAs are nouns ("Pricing", "Docs") or lower-commitment verbs ("See a demo").
- The button color has one job: to be findable. The copy does the persuasion.
- Avoid first-person-singular CTAs ("Get my plan"); users are not writing notes to themselves.
- Test button copy separately from page copy; button copy is the cheapest test with the largest delta.

Bad primary CTA: "Learn more"
Good primary CTA: "Start the 14-day trial"
Good friction line: "No credit card. Works with 1–50-person teams."

## Social proof copy

Evidence earns headlines. Use the shortest specific proof available.

### Logo bars

- Use only logos granted in writing.
- Group by segment or by size so the story is legible: "Trusted by teams at Stripe, Linear, Vercel, Shopify".
- Six to twelve logos; more than twelve becomes wallpaper.

### Stats

- Prefer specific, verifiable numbers. "Cuts incident-acknowledgement time by 43% at Stripe" beats "Massive time savings".
- Include the source and the basis. A number with no denominator is not evidence.
- Round responsibly — "43%" beats "43.2%".

### Testimonials

Each testimonial must do three things: name a specific objection, attach to a real person, and close with a concrete outcome.

Weak:

> "Great product, highly recommend." — Jane D.

Strong:

> "Before Flint, the on-call engineer got paged eleven times a night. After, twice. The team sleeps again."
> — Priya Menon, Head of Platform, Linear

Collection patterns:

- Pair each product section with a testimonial that speaks to that section's promise.
- Harvest testimonials from sales calls and support tickets; they are more specific than requested ones.
- Always ask for permission in writing; retain the exact wording.
- Include a photo, a full name, a role, and a company logo. Anonymous testimonials read as invented.

### Case studies

Follow the Before → Intervention → After → Quote skeleton:

- Before — the measurable pain, quantified.
- Intervention — what the customer changed, named concretely.
- After — the measurable outcome, with the metric, the time frame, and the source.
- Quote — a single pull quote from a named operator at the customer.

Case studies earn 800–1,500 words. Fewer than 800 words is a testimonial, not a case study.

## Navigation and information-architecture copy

Menu labels are copy. Bad labels hide good products.

- Use nouns for destinations, verbs for actions. "Pricing" is a destination; "Start trial" is an action.
- One or two words per label, sentence case, consistent across nav and footer.
- Labels match user vocabulary, not internal org structure. "Docs" beats "Knowledge center"; "Help" beats "Customer experience resources".
- Dropdown menus group by purpose, not by team. Each group has a one-word noun header.
- Breadcrumbs reflect the URL and content hierarchy, not the navigation menu. Separator is `›` or `/`; the last segment is not a link.
- Search placeholder text names what is searched ("Search docs", "Search articles", not "Search…").
- "Log in" and "Sign up" are the canonical forms; be consistent across the site.
- Skip links use the landing location, not a verb: "Skip to main content".

Bad menu label: "Solutions"
Better: "For engineering" / "For design" / "For sales" — any one of which is scannable.

## Notifications, toasts, banners

- Toast — ephemeral, dismissable, acknowledges an action ("Invoice sent").
- Banner — persistent, system-level ("Scheduled maintenance on April 28, 02:00 UTC").
- Modal — blocking, requires a decision ("Delete this project? This cannot be undone.").

Rules:

- Severity signals mean something. Red for failure or destructive action, amber for warning, blue for informational, green for success. Never use color alone; pair with an icon and text.
- Every notification names the object, the action, and — if something can still go wrong — the state. "Draft saved 5 seconds ago" beats "Saved".
- Do not stack notifications. Queue them or replace them.
- Persistent banners must be dismissable; dismissal is remembered per user.

Bad toast: "Success!"
Good toast: "Payment method updated. Next invoice charges Visa •• 4242."

## Cookie and consent banner copy

Consent banners are legally material and, in most jurisdictions, the first interaction the site has with a visitor.

- State plainly what cookies do. "These cookies remember returning visitors and help improve the site." is clearer than legalese.
- Offer three symmetric buttons: "Accept all", "Reject all", "Manage preferences". Reject must be as easy as Accept; pre-selected checkboxes on non-essential categories are a dark pattern and may violate GDPR.
- Link the full cookie policy below the buttons, not in place of them.
- Never block page content behind an unresolved banner unless legally required.
- On "Manage preferences", group cookies by purpose (strictly necessary, analytics, marketing, personalization). Strictly-necessary is disabled but explained; the rest default to off.

## Power words, specificity, and sensory language

Abstract writing is the default failure mode. Concrete language fixes it.

- Prefer sensory verbs. "Drown", "ship", "sleep", "burn", "patch", "ping". Avoid managerial verbs. "Leverage", "utilize", "operationalize".
- Prefer specific nouns. "Friday releases", "on-call engineer", "pull request", "Stripe Dashboard". Avoid abstract collectives. "Stakeholders", "resources", "solutions".
- Use numbers wherever possible. "Cuts deploy time from 14 minutes to 90 seconds" beats "Much faster deploys". Numbers set expectations; adjectives do not.
- Concrete beats abstract. "A $2,400 invoice" beats "A significant bill".
- Power words that earn their keep when used sparingly: "free", "new", "now", "because", "proven", "only", "guaranteed". Overuse degrades trust.
- Avoid these without evidence: "revolutionary", "world-class", "best-in-class", "industry-leading", "cutting-edge", "next-generation", "game-changing", "disrupt", "synergy".

## Reading level and sentence discipline

A brand voice only lands if the reader can read it.

- Flesch-Kincaid grade targets by surface:
  - Marketing and landing — grade 7–9.
  - Onboarding and in-product — grade 8–10.
  - Documentation — grade 10–12.
  - Legal — as low as the compliance team will allow; 10–12 is the ceiling.
- Sentence length — average 14–18 words, cap at 25. A sentence over 30 words hides two sentences.
- Paragraph length — average 2–4 sentences in marketing copy; up to 5 in documentation.
- Active voice by default. Passive only when the actor is unknown, irrelevant, or deliberately obscured.
- One idea per sentence. If "and" or "which" is doing structural work, split.
- Read every sentence aloud. If it stumbles, rewrite.

## Clarity over cleverness

Edit every paragraph through the Hemingway filter:

- Cut adverbs.
- Replace passive voice with active.
- Break long sentences.
- Replace abstract nouns with concrete ones.
- Delete the first sentence; most introductions are warm-ups.

Read every sentence out loud. If it stumbles, it fails.

## Editing passes

Edit in passes. A single read-through trying to fix everything fixes nothing.

- Pass 1 — truth. Every claim defensible. Every number sourced. Every testimonial attributable. Every outcome achievable by a typical user, not a power user.
- Pass 2 — specificity. Every abstract noun replaced with a concrete one. Every adverb either cut or replaced with a stronger verb. Every generic category replaced with a named thing.
- Pass 3 — rhythm. Read aloud. Fix stumbles. Vary sentence length. Short sentences for emphasis; longer ones for flow.
- Pass 4 — voice. Hold against the brand doc. "But not" modifiers still apply.
- Pass 5 — cut. Remove 20%. Anything that does not carry load goes. Delete the first sentence of every paragraph and reinstate only if the second sentence no longer makes sense.

Running the five passes in order takes roughly as long as a single undisciplined edit and produces visibly different copy.

## Copy measurement and testing

Copy is the cheapest variable to change and the most frequently under-instrumented.

- Form a hypothesis before testing. "Changing the primary CTA from 'Sign up' to 'Start free trial' will raise clickthrough by at least 5% because it names the outcome and reduces perceived commitment." A test without a hypothesis is a vibe.
- Test the single highest-traffic element first — usually the hero CTA or the pricing-page subhead.
- Instrument beyond clickthrough. CTR is cheap to move and often misleading. Track the downstream conversion (signup → activation, signup → first invoice, trial → paid) to catch tests that move the wrong users.
- Run tests long enough for a reliable result. Statistical rigor requires sufficient sample size; a 48-hour A/B test is noise.
- Segment results by traffic source and device. Paid-search visitors and organic visitors behave differently; copy that wins for one may lose for the other.
- Qualitative validation — run the winning variant past five users in a 5-second test. If they cannot repeat the promise, the test won a local optimum.
- Track reading depth on blog content (scroll depth, time on page), not pageviews alone.

## SEO in the AI-search era
Google Search increasingly includes AI features alongside classic results. For AI Overviews and AI Mode, Google documents the same foundational SEO practices as Search overall: the page must be indexed, eligible to appear with a standard snippet, and compliant with Search policies. Google does not document additional technical requirements for appearing as a supporting link.

### Content and E-E-A-T

- Write for genuine expertise, experience, authoritativeness, and trust (E-E-A-T). Cite sources, name authors, show credentials.
- Structure content so that a paragraph, list, or table can answer one clear question without surrounding context.
- Use descriptive, unique, question-aligned headings. Write them as the question the user is asking, not a category label.
- Keep Core Web Vitals in the green; page experience affects rankings.
- Maintain a stable, readable URL structure. Avoid query-string-heavy paths for evergreen content.
- Write meta descriptions as short, truthful summaries, not keyword stuffing.
- Ensure crawlability; do not gate primary content behind client-side JavaScript without a server-rendered or prerendered path.
- Make important content available in text form.
- Use internal links so important pages are discoverable from the rest of the site.

### Structured data (JSON-LD)
Structured data helps Google understand page content and can enable rich results when the page type and properties qualify. It is not a substitute for crawlability, indexing, or visible page content. JSON-LD is Google's recommended format because it is easier to implement and maintain at scale.

Implement schema at the page-type level so every new page automatically gets consistent markup, and make sure the markup matches the visible text on the page.

Key schema types by site type:

| Type | Use for | Notes |
| --- | --- | --- |
| `Organization` | Homepage | Brand and entity details |
| `BreadcrumbList` | All inner pages | Breadcrumb display where Google chooses to show it |
| `Article` / `BlogPosting` | Blog posts | Article-related search features where the page qualifies |
| `Product` | Product pages | Price, availability, and rating data where supported |
| `LocalBusiness` | Physical locations | Local entity information |
| `FAQPage` | Pages with one authoritative answer per question and visible FAQ content | Rich-result availability is currently limited to well-known, authoritative government-focused and health-focused sites |

- Do not depend on `HowTo` markup for a Search rich result. Google deprecated How-to rich results in Search in 2023.

Example Article schema:

```html path=null start=null
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Page title here",
  "author": { "@type": "Person", "name": "Author Name", "url": "https://example.com/team/author" },
  "publisher": { "@type": "Organization", "name": "Site Name", "url": "https://example.com" },
  "datePublished": "2026-04-23",
  "dateModified": "2026-04-23",
  "image": "https://example.com/og-image.jpg",
  "mainEntityOfPage": { "@type": "WebPage", "@id": "https://example.com/article-slug" }
}
</script>
```

Use `FAQPage` only when the page contains a single authoritative answer to each question and the same content is visible on the page. Google allows the answer to be hidden behind an expandable section that the user can open. Do not rely on FAQ markup as a general-purpose SERP-expansion tactic.

```html path=null start=null
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is the question?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "A self-contained 2–4 sentence answer that makes sense without surrounding context."
      }
    }
  ]
}
</script>
```

Write each answer as a complete, self-contained answer. The markup must match the visible question and answer text on the page.

For AI features, treat structured data as clarification rather than as a dedicated AI citation mechanism. Google documents no additional technical requirements for AI Overviews or AI Mode beyond standard Search eligibility.

### Sitemaps and crawlability

- Submit an XML sitemap to Google Search Console and Bing Webmaster Tools.
- Include only canonical, indexable URLs. Exclude redirected pages, `noindex` pages, and error URLs.
- Use `lastmod` dates accurately — stale or auto-generated dates signal distrust to crawlers.
- Robots.txt controls crawling, not indexing. Never block pages you want indexed, even accidentally.

### Preview controls

- Use `nosnippet`, `data-nosnippet`, `max-snippet`, or `noindex` when Search preview controls must limit what appears in Search snippets or AI features.
- Validate preview-control changes in URL Inspection and allow time for recrawling.

### Validation and monitoring

- Validate structured data using Google's Rich Results Test before deploying.
- Monitor Search Console rich-result and performance reports for schema errors and traffic changes.
- Search Console reports AI-feature traffic inside the overall `Web` search type.
- Track impressions, clicks, and CTR before and after changes. Eligibility for a rich result does not guarantee that Google will show it.

## Inclusive copy

- Avoid cultural idioms, slang, and region-specific references unless the audience is explicitly that region.
- Do not assume gender. Use singular "they" unless a specific person is named.
- Prefer active construction that credits the user: "You saved your changes," not "Changes were saved."
- Avoid ableist language ("crazy," "insane," "blind to") and violent metaphors ("kill it," "hit a home run") in product copy; they are lazy and alienating.

## A final test

Read the finished copy aloud to someone outside the project. If they can repeat the promise in their own words, the copy works. If they restate it with the brand's marketing jargon, rewrite.
