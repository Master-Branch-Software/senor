# Discovery and Communication

Generating a website well depends on having the right inputs. A brief that omits the audience, the brand voice, or the success metric will produce generic output no matter how good the downstream process is. A request treated as complete when it is actually ambiguous produces confident wrong answers. This chapter defines what to ask, when to ask it, and when to proceed without asking.

The goal is not to interrogate the user. The goal is to be decisive where decisiveness is safe and to surface ambiguity where guessing would waste their time.

## Principles

- Ask when the answer materially changes the output. Do not ask for confirmation on questions with a strong default in this guide.
- Ask one focused question at a time when the answer unblocks the next step. Batch related questions when the user is in planning mode.
- Offer choices, not open-ended prompts. "Which of A, B, or C fits best?" produces better answers than "What do you want?"
- State the assumption before acting on it when asking is not possible. "Assuming this is a public marketing site with no login, scaffolding Astro. Say otherwise before routes are generated."
- Stop asking once enough inputs exist to start. Perfectionism on the brief is its own failure mode.
- Surface trade-offs explicitly. Users cannot decide between options they are not shown.
- Never silently violate a non-negotiable. If a user request conflicts with accessibility, security, or performance baselines, say so before proceeding.

## The minimum brief

These inputs are required before meaningful design or code begins. If any are missing, ask. Do not improvise them.

- Audience. One sentence describing the person and the job to be done. Not a demographic.
- Primary action. One verb-noun pair. "Book a demo." "Start a trial." "Read the article."
- Differentiator. One sentence grounded in something true. If the user cannot state it, raise that as a content problem, not a design problem.
- Target feel. One adjective chosen deliberately (sharp, warm, calm, confident, playful, editorial, brutal).
- Success metric. What does success look like at 30 and 90 days.
- Deadline and scope. When does it ship, and what belongs in v1 versus later.
- Existing brand assets. Logo, colour palette, typography, voice guide, prior site. If any exist, request them up front.
- Content status. Is copy written, outlined, or to-be-written. Are images available and licensed.
- Technical constraints. Existing hosting, stack, team skills, CMS preference, required integrations.
- Compliance. GDPR, CCPA, HIPAA, SOC 2, WCAG target level, data residency, regional payment rules.
- Internationalization. Day-one locales. If none, confirm English-only is acceptable.
- Expected traffic and scale. "A few hundred daily" versus "tens of thousands concurrent" changes the entire stack.

If the answer to any of the above is "whatever you recommend," treat that as permission to apply the defaults in this guide and state the chosen default in writing.

## Staged disclosure

Do not demand every input at once. Ask at the stage where the answer becomes actionable.

1. Brief (who, what, why, feel, success). Before any design or code.
2. Voice and tone. After the brief. Before any copywriting.
3. Sitemap. After voice. Before any page-level work.
4. Copy per page. After the sitemap.
5. Design tokens (colour, type, spacing, radius, shadow). After the brief and voice, before any component or page.
6. Components. After tokens.
7. Pages. After components.
8. Accessibility and performance passes. Continuous during pages, not a gate at the end.
9. Launch plan. After pages exist on staging.

When the user says "just build it," confirm the brief items, announce the staged plan, and then proceed without re-asking at each sub-stage unless something genuinely blocks progress.

## Defaults to apply without asking

The following are answered by this guide and do not require user input. State them as applied, not as questions.

- Body text size, line height, line length (16 px / 1.5 / 60–75 ch).
- Focus indicator styling (`:focus-visible`, 2 px outline, ≥ 3:1 contrast).
- Colour-contrast thresholds (WCAG 2.2 AA unless a higher target is required by the brief).
- Spacing scale (4/8/12/16/24/32/48/64/96 px).
- Typography scale (modular ratio 1.125–1.25 for most sites, 1.333+ for editorial).
- Reset CSS and `@layer` order (reset, base, components, utilities).
- `prefers-reduced-motion` handling.
- Image defaults (`width`, `height`, `alt`, `loading`, `decoding`, `fetchpriority` on LCP).
- Logical properties over physical (`margin-inline-start`, `padding-block`, `inline-size`).
- Code style (two-space indent, kebab-case files, `visible` over `isVisible`).
- Testing stack for Vite projects (Vitest + Testing Library + Playwright + MSW).
- Lint and format config (ESLint + Prettier or Biome).
- Security headers baseline (CSP with nonces, HSTS, `X-Content-Type-Options: nosniff`, `Referrer-Policy`, `Permissions-Policy`).

## When to ask before choosing

Ask before committing to any of these, and offer the default recommendation in the same message so the user can accept with a single word.

- Framework (Astro, Next.js, SvelteKit, Remix / React Router v7, no framework).
- Hosting (Vercel, Netlify, Cloudflare, Railway, Fly.io, self-hosted).
- CMS (none, MDX in the repo, Sanity, Payload, Contentful, Storyblok, Directus).
- Database (none, Postgres via Neon or Supabase, SQLite via Turso, self-hosted Postgres).
- Auth provider (Clerk, Auth.js, Supabase Auth, Better Auth, WorkOS, enterprise SSO).
- Payments (Stripe, Paddle, LemonSqueezy, none).
- Analytics (Plausible, Fathom, PostHog, GA4, none).
- Email provider (Resend, Postmark, SES, SendGrid).
- Brand archetype when no brand document exists.
- Any irreversible decision: domain, organization names, DB schema for user-owned data, API contract shape, URL slugs that will be indexed.

Format of a good pre-commit question:

> Recommended stack: Next.js + Postgres (Neon) + Clerk + Stripe + Resend + Vercel. This fits a small SaaS with standard auth and payments. Accept this, or call out changes.

## Communicating during execution

- State assumptions before acting. "Assuming no logged-in area."
- Surface trade-offs at decision points. "Astro means opting into client-side React per component; if rich interactivity is throughout the site, Next.js fits better."
- Announce at stage boundaries, not at every file write. "Brief captured. Moving to voice and tone." Progress noise at every step trains the user to stop reading.
- Preserve the user's exact copy, brand names, and quoted text. Do not paraphrase brand copy into generic marketing prose.
- When the user provides a style guide, brand document, or existing code, treat it as the source of truth. Flag conflicts with this guide and let the user decide.
- When a user requests something this guide recommends against (pure black on white, `vw` for font-size, `<div onClick>`, unlabeled inputs), raise the concern once, then follow the user if they confirm after hearing it.
- Never silently skip a non-negotiable. If a request would produce an inaccessible or insecure result, name the specific rule being violated and propose the compliant alternative.

## Red flags in user requests

Some phrases consistently produce worse output unless clarified. When one appears, ask a narrow follow-up before proceeding.

- "Make it pop" / "modern" / "clean." Ask for a reference site and a target feel adjective.
- "Like [large brand]." Ask which specific properties to emulate (layout, colour, motion, type, voice). Do not copy wholesale; copy violations and design lawsuits start here.
- "SEO-optimized." Ask for target keywords, the audience's query intent, and existing ranking content.
- "Responsive." Confirm whether this means "usable on phones" (baseline) or "a distinct mobile design" (real work).
- "Simple" paired with a forty-item feature list. Confirm which features are actually v1.
- "Enterprise" / "scalable" for a small audience. Confirm the real user count. Do not over-engineer for imagined scale.
- "AI-powered" with no stated use case. Ask what the AI should do for the user, concretely, with an input and an output.
- "Just the basics." Ask what counts as basic for this brand.
- "Like before, but better." Ask for the prior version and a list of specific changes.
- "We need it fast" with no deadline. Ask for a date and a hard requirement list.
- "High-converting." Ask what action is being converted and what the current conversion rate is.

## Scope and trade-offs

When the feature list exceeds the deadline or the budget, do not silently choose. Offer explicit options.

- "v1 with A, B, C shipping by the deadline; D and E follow in v1.1."
- "All of A–E, with the deadline pushed two weeks."
- "All of A–E by the deadline with reduced depth on each feature."
- "Drop E entirely, since its core value overlaps with A."

Let the user choose. Half-building every feature to avoid the conversation is the default failure mode.

## Checkpoints

Pause and show progress at these points. Do not continue past a checkpoint without sign-off on anything irreversible.

- Brief written. Confirm before voice work.
- Voice and tone written. Confirm before copywriting.
- Sitemap written. Confirm before wireframes.
- Design tokens produced with contrast audit. Confirm before component work.
- Scaffold created with primitives and accessibility defaults. Confirm before the first page.
- First page complete. Confirm before rolling the same pattern across the rest.
- Accessibility audit clean (automated and manual). Confirm before launch.
- Performance audit clean (Core Web Vitals within targets in the lab). Confirm before launch.
- Staging deploy live with smoke tests passing. Confirm before pointing the production domain.

## Non-frontend concerns to clarify

A website is rarely just a frontend. When any of these matter, confirm them explicitly before building.

- Data model. What entities exist, what relationships matter, who owns each record.
- User roles. Who logs in, what each role can do, whether organizations or teams exist as concepts.
- Content workflow. Who writes, who approves, who publishes, how often, and whether previews or staged releases are needed.
- Payments. One-time, subscription, usage-based, or metered; which markets; tax handling; refunds and disputes.
- Email. Transactional only, or also marketing; expected volume; deliverability and bounce handling; compliance (CAN-SPAM, CASL, GDPR opt-in).
- File uploads. Maximum size, allowed types, storage target (S3, R2, Cloudinary, Sanity assets), virus scanning, retention.
- Search. Is search a product feature or an afterthought; what is indexable; expected query volume; multilingual needs.
- Notifications. In-app, email, SMS, push; which events trigger each; user preference controls.
- Background work. What runs on a schedule; what runs on events; whether failures need retry and dead-letter handling.
- Observability. Error tracking, uptime monitoring, product analytics, log retention, on-call rotation.
- Legal and privacy. Privacy policy, terms of service, cookie banner if EU or UK traffic is expected, DPA if any third-party processor touches personal data, data deletion and export for GDPR/CCPA.
- Handoff. Who maintains the site after launch, and whether documentation or a training session is required.

## What not to ask

- Anything already answered in the brief.
- Anything already defaulted by this guide (see "Defaults to apply without asking").
- Confirmation of standard engineering practice (using `<button>` over `<div onClick>`, hashing passwords, validating input on the server).
- Minute styling choices the agent should decide and justify in one sentence (exact shade of an accent colour, icon weight, border radius in pixels).
- The same question twice in a session.
- Open-ended preference questions when a recommended default exists. Offer the default and accept silence as agreement.

When the user skips a question, proceed with a stated assumption and continue. When they answer vaguely, apply the strongest reasonable interpretation and announce it. Blocking on the user when action is possible is worse than acting on a clearly stated assumption.

## Handling ambiguity and contradiction

- Conflicting inputs from the user (brief says "warm" but the reference is a brutalist site): restate both, ask which wins.
- User-provided assets that violate this guide (a brand palette with 1.9:1 contrast): surface the issue, propose a compliant adjustment, let the user decide.
- Two users contradicting each other in the same thread: name both positions, ask who decides.
- A request that implies a different stack than already agreed: flag the implication before acting.

## Closing the loop

At launch, produce a short post-launch note for the user: what was built, what was explicitly deferred, what assumptions were made, and what should be reviewed at 30 and 90 days against the success metric in the brief. This is the artifact that turns a one-shot build into a long-term relationship.

## The shortest discovery

If only three questions can be asked before starting:

1. Who is this for, and what is the one action they should take?
2. What feeling, and what success metric.
3. What constraints — deadline, budget, existing stack, compliance, content status.

Everything else can follow from those three plus the defaults in this guide.
