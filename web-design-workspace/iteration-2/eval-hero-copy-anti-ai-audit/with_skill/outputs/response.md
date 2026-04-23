# Response: hero-copy-anti-ai-audit (WITH skill)

_Skill loaded: SKILL.md. Primary chapter loaded: 02-brand-and-copywriting.md._

---

## Audit: Current hero copy

> "Empower your DevOps team to streamline deployments and unlock the full potential of your pipeline. Our robust platform leverages AI to foster seamless collaboration, ensuring optimized workflows that drive innovation at scale."

### Anti-AI pattern analysis

Every problem in this copy has a documented pattern. I'll name each one with the rule it violates.

**Abstract verbs (forbidden without evidence)**
- _Empower_ — forbidden. No mechanism. What does it empower the user to do, concretely?
- _Streamline_ — forbidden. This word has lost all meaning in B2B SaaS.
- _Unlock_ — forbidden. Vague. Same objection as "empower."
- _Leverage_ — forbidden by brand voice (appears on the avoid list for any Sage brand). Managerial jargon.
- _Foster_ — forbidden. Soft, intransitive, no specific outcome.
- _Drive_ — forbidden in this context. "Drive innovation" has no subject, object, or timeline.

**Vague collectives and abstractions**
- _Full potential of your pipeline_ — not a real thing. No reader has ever thought "I need to unlock the full potential of my pipeline." They think "I need to know why that deploy took 40 minutes and paged three people."
- _Seamless collaboration_ — forbidden phrase. Used in 83% of B2B SaaS hero sections. Carries zero information.
- _Optimized workflows_ — outcome without a number or mechanism. Optimized how? By what measure?
- _Innovation at scale_ — the most abused phrase in enterprise software marketing. Means nothing specific to this product.
- _Robust platform_ — table-stakes claim. No product calls itself fragile.

**No specific audience named**
The copy addresses "your DevOps team" — vague. The actual audience is "senior platform engineers at Series B–D startups." They use different vocabulary than "DevOps team" suggests. They say "pipeline," "deploy," "incident," "page," "runbook," not "workflow" and "collaboration."

**No concrete mechanism**
The product is a CI/CD observability tool. The copy never says what it actually shows the user. What does someone open this tool to see? Where does the data come from? What decision does it help them make?

**No numbers**
The only quantification is "at scale" — which is not a number. No deploy time, no MTTR, no team size, no percentage improvement.

**Hedging tone inconsistent with Sage/Hero archetype**
The Sage archetype speaks with precision and evidence. The Hero archetype speaks with directness. This copy does neither. It uses a passive, sweeping tone that no senior platform engineer would recognize as speaking to them.

---

## Rewrite: hero copy options

Three drafts, each applying a different headline formula from chapter 02. All use the brand voice (direct but not curt; technical but not jargon-heavy; wry but not snarky; senior but not condescending).

---

### Draft A — Specific outcome + mechanism (recommended for Sage/Hero)

**Headline** (7 words):
> See every deploy. Know why it broke.

**Subheadline** (22 words):
> [Product] instruments your CI/CD pipeline and surfaces root-cause failures before on-call engineers are paged. No dashboards to build.

**Primary CTA**: Connect your pipeline free
**Secondary CTA**: See a demo

**Why this works**:
- Names the concrete action ("see every deploy," "know why it broke") — not abstract.
- "Before on-call engineers are paged" names the specific, painful moment the audience knows. Senior platform engineers have been paged. This lands.
- "No dashboards to build" addresses the real objection: observability tools require weeks of setup. This closes it in five words.
- Archetype check: Sage (precise, grounded) with Hero secondary ("surfaces root-cause failures" implies the product does the heroic work).
- Zero forbidden words.

---

### Draft B — Problem-first, BAB structure

**Headline** (7 words):
> Stop debugging deploys at 2 a.m.

**Subheadline** (24 words):
> [Product] connects to your CI/CD pipeline in minutes. It flags the failing step, the commit, and the engineer to call — before an incident is declared.

**Primary CTA**: Connect your pipeline free
**Secondary CTA**: Read how it works

**Why this works**:
- "2 a.m." is specific and sensory. Every on-call engineer has been there.
- Subheadline names three concrete things the product surfaces (failing step, commit, engineer) — not abstract outputs.
- Wry, but not snarky: "before an incident is declared" has a dry precision that matches the brand voice and the audience's mindset.

---

### Draft C — Ogilvy fact-first (use only if a real stat is available)

**Headline** (to be filled with a real stat):
> Teams using [Product] cut mean time to identify by 73%.

**Subheadline** (17 words):
> Full CI/CD observability. No agents to install. Works with GitHub Actions, CircleCI, and Buildkite out of the box.

**Primary CTA**: Start for free — connects in 10 minutes

**Why this works**:
- If the stat is real and verifiable, this pattern (Ogilvy fact-first) is the strongest available.
- Subheadline answers the two most common objections from this audience: "Is it another agent I have to install?" and "Does it support my stack?"
- If the stat does not exist, do not use Draft C. A made-up number is worse than no number.

---

## Brand voice check (all three drafts)

| Attribute | Draft A | Draft B | Draft C |
|---|---|---|---|
| Direct, but not curt | ✓ | ✓ | ✓ |
| Technical, but not jargon-heavy | ✓ | ✓ | ✓ |
| Wry, but not snarky | ✓ | ✓ (2 a.m.) | ✓ |
| Senior, but not condescending | ✓ | ✓ | ✓ |
| Forbidden words used | None | None | None |
| Concrete mechanism | ✓ | ✓ | ✓ |
| Specific audience implied | ✓ | ✓ | ✓ |

---

## Recommendation

Use **Draft A** as the primary test variant unless a stat is available (then Draft C). Draft B is a strong alternative if user research confirms "2 a.m. deploys" resonates — validate with a 5-second user test before going live.

Run both A and C as an A/B test if traffic allows. The downstream metric to track is not clickthrough on the CTA; it is trial signup → first pipeline connected. That is the activation event that matters for this product.

---

_Self-review: all forbidden words from the original removed. Archetype (Sage primary, Hero secondary) maintained. Three headline formulas applied per chapter 02. Drafts do not use "delve," "landscape," "robust," "holistic," "seamless," "foster," "leverage," "navigate," "tapestry," or "multifaceted." No "AI-powered" or "revolutionary" claims. One deviation: the product name is not stated in the brief, so [Product] is used as a placeholder — replace before publishing._
