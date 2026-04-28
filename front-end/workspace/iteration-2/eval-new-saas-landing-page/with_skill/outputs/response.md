# Response: new-saas-landing-page (WITH skill)

_Skill loaded: SKILL.md. Primary chapter loaded: 15-discovery-and-communication.md._

---

Thank you for that context — you've already answered a lot of the minimum brief. Before beginning stage 1, I have a few targeted gaps to close. I'll ask one at a time.

**Question 1 of 4 (primary action):**

The brief describes what the product does, but I need the single action the landing page should drive. Which of these fits best?

- A. **Start a free trial** (self-serve signup)
- B. **Book a demo** (sales-assisted conversion)
- C. **Join a waitlist** (early access, not yet live)

Whichever you choose, that becomes the primary CTA and every section is written to produce that one action.

---

_Assuming you answered "Start a free trial" — I'll proceed on that default and note it in brief.md. Say otherwise before copy is written._

---

## Stage 1 output: `brief.md` and `constraints.md`

### brief.md

```
Product: [working name TBD]
Tagline candidate: "Time tracked. Reports written. No timesheets."

Audience:
  Engineering managers at remote-first companies, 50–500 employees.
  Job-to-be-done: understand where project time is going without
  forcing engineers to fill timesheets every Friday.

Primary action: Start a free trial (assumed — confirm before copy).

Differentiator:
  AI auto-categorizes time by project and client from existing tool
  activity (Jira, GitHub, Slack). Managers get reports; engineers
  fill nothing.

Target feel: Calm and professional. (User's exact words.)
  Adjective for audit: calm.

Success metric: Not stated. Recommended default — 30-day target:
  X trial signups from organic + paid; 90-day target: Y% trial-to-paid
  conversion. Confirm before launch.md.

Deadline: 3 weeks to live.

Integrations confirmed: Jira, GitHub, Slack.

Brand assets available: Logo + 3 product screenshots.

Content status: Copy to be written (this project). Images: 3
  screenshots available and licensed.

Compliance: None required (user confirmed).

Locales: English only (user confirmed).

Stack preference: Not stated (will ask at stage 6).

Expected traffic / scale: Not stated. Assuming low-to-medium
  marketing site traffic. Will confirm at stack stage.
```

### constraints.md

```
Hard constraints
- 3-week deadline to live.
- English only.
- No compliance requirements (GDPR banner still needed if any EU
  traffic is expected — will flag at scaffold stage).
- Must integrate with Jira, GitHub, Slack on the product side
  (landing page demonstrates these; does not implement them).

Soft constraints / open decisions (to resolve at later stages)
- Primary CTA: assumed "Start a free trial" — confirm.
- Success metric: not stated — confirm before launch.md.
- Stack: not stated — will recommend at stage 6.
- Browser matrix: will default to Tier 1 (last 2 versions of
  Chrome, Firefox, Safari, Edge + iOS Safari + Chrome Android)
  unless stated otherwise.
- Dark mode: not stated — will apply system-preference-aware
  tokens by default.

Non-negotiables applied silently (no need to ask)
- Every <img> gets width, height, meaningful alt.
- LCP image not lazy-loaded; fetchpriority="high".
- Body text ≥ 16 px, line-height 1.5, line length 60–75 ch.
- WCAG 2.2 AA contrast: 4.5:1 body, 3:1 large text and UI.
- Visible :focus-visible on every focusable element.
- prefers-reduced-motion honored.
- Native HTML first (<button>, <dialog>, <details>).
- font-display: swap with metric-matched fallbacks.
- No positive tabindex.
- Two-space indent, kebab-case files, visible not isVisible.
- .DS_Store in .gitignore.
```

_Self-review: brief.md is missing a confirmed primary action and success metric. Both flagged. Proceeding to stage 2 (voice) using the assumptions stated above — upstream fix required before copy is written if the assumption is wrong._

---

**Question 2 of 4 (voice):**

Before I write `brand.md`, I need to anchor the archetype. Based on the brief — calm, professional, targeted at engineering managers, AI-driven but not flashy — I'd recommend:

**Sage primary, Creator secondary.** Sage communicates measured intelligence and trust (the brand explains, it doesn't sell). Creator nods to the product's automation angle without overselling it.

Does that fit, or would you adjust?

---

_Assuming Sage/Creator — proceeding on that default._

## Stage 2 output: `brand.md`

```
Archetype: Sage (primary), Creator (secondary).

Voice attributes:
- Direct, but not curt. Sentences are short. No padding.
- Technical, but not jargon-heavy. "Time by project" not
  "granular effort allocation telemetry."
- Calm, but not flat. Conviction without exclamation marks.
- Confident, but not arrogant. Claims are grounded; no
  "leading," "best-in-class," or "revolutionary."

Tone map (dial positions per surface):
- Hero / above fold:   mid formality, warm, low urgency, no play.
- Feature sections:    mid formality, neutral warmth, low urgency, no play.
- Onboarding:          low formality, warm, low urgency, play permitted.
- Error states:        mid formality, warm, low urgency, no play.
- Billing:             high formality, neutral, low urgency, no play.

Vocabulary the brand uses:
  time, project, client, report, categorize, engineering team,
  manager, visibility, automatically, on-call (secondary use),
  ship, deploy, sprint, pull request, ticket.

Vocabulary the brand avoids:
  empower, unleash, leverage, disrupt, revolutionize, synergy,
  streamline, seamless, cutting-edge, next-generation,
  game-changer, AI-powered (as a feature name), holistic,
  robust, unlock potential.
```

_Self-review: no deviations. Upstream brief is still missing confirmed primary action — brand.md depends on it being "Start a free trial." Flagged._

---

**Question 3 of 4 (sitemap):**

For a 3-week deadline with one product and English-only, I recommend a tight v1 scope:

- `/` — Home (hero, features, social proof, pricing preview, CTA)
- `/pricing` — Pricing (tiers, FAQ)
- `/integrations` — One page listing Jira, GitHub, Slack (and roadmap integrations if any)

Everything else (blog, docs, about, changelog) deferred to v1.1.

**Accept this scope, or add/remove pages before wireframes?**

---

_Assuming accepted — proceeding._

## Stage 3 output: `sitemap.md`

```
v1 pages
1. / (Home)
   Goal: drive trial signups from cold traffic.
   Primary action: Start a free trial.
   Evidence types: product screenshots, integration logos, stat
   or testimonial (to source).

2. /pricing
   Goal: convert visitors already sold on the product.
   Primary action: Start a free trial (Starter/Pro tiers).
   Evidence: feature comparison table, FAQ.

3. /integrations
   Goal: reassure prospects the tool works with their stack.
   Primary action: Back to home / Start a free trial.

Deferred to v1.1
- /blog
- /docs
- /about
- /changelog
- /security
```

---

**Question 4 of 4 (stack):**

Recommended stack for a 3-week deadline, marketing site, no login area, no CMS required yet:

> **Next.js (App Router) + Tailwind CSS + Vercel. No CMS for v1 — copy lives in the repo as MDX or JSX. No auth, no database.**

This gives you a fast static site, good image optimization via `next/image`, easy deployment, and room to add a CMS later without rewriting.

**Accept, or call out changes?**

---

_Assuming accepted. At this point I have enough to begin wireframes (`wireframes/home.md`), then copy, then tokens, then scaffold. Each produces a named artifact; I'll pause for your review at each stage boundary._

**Ready to proceed to wireframes? Or do you want to confirm/correct any of the four open assumptions first (primary action, success metric, archetype, stack)?**
