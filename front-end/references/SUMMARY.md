# Summary

Condensed guide. Load alone when context tight; full chapters for depth.

## Premise

Website serves specific person, specific goal, specific brand voice. Every choice — color, word, spacing, motion — evaluated against those three. Craft input; beauty output.

## Non-negotiables

- Every `<img>` has `width`, `height`, and meaningful `alt`.
- The likely LCP image is never lazy-loaded. Use `fetchpriority="high"` on the `<img>` and preload it when the browser would otherwise discover it late.
- Body text ≥ 16 px, line-height 1.5, line length 60–75 characters.
- WCAG 2.2 AA contrast: 4.5:1 body text, 3:1 large text, 3:1 UI and focus rings.
- Visible `:focus-visible` on every focusable element.
- `prefers-reduced-motion` is honored.
- Native HTML first: `<button>`, `<dialog>`, `<details>`, `popover` attribute before custom.
- Fonts use `font-display: swap` with matched-metric fallbacks or `font-display: optional`.
- No positive `tabindex`.
- Two-space indent, kebab-case files, `visible` not `isVisible`, logical properties, `.DS_Store` in `.gitignore`.

## 01 — Philosophy and psychology

- 4 questions before designing: who, primary action, why trust, what feeling.
- Laws of UX: Jakob (conventions), Hick (fewer choices), Fitts (target size), Miller (7±2), Doherty (<400ms), Peak–End (hero+final states), Zeigarnik (progress).
- Nielsen's 10 heuristics; Gestalt (proximity, similarity, continuity, closure).
- Cialdini's 7 principles: reciprocity, commitment, social proof, authority, liking, scarcity, unity.
- Don Norman 3 levels: visceral, behavioral, reflective.
- 5-sec test: what is this, who for, what to do.

## 02 — Brand and copywriting

- Document brand in writing: audience, offer, reason-to-exist, personality, archetype, voice with "but not" modifiers, use/avoid word lists.
- Pick one Jung archetype (12 total); mixing >2 = incoherence.
- Voice fixed; tone adapts on 4 dials (formality, warmth, urgency, playfulness) per context (hero, onboarding, error, destructive confirm, outage, churn, legal).
- Voice-of-customer research: harvest 150–300 verbatim quotes from interviews, sales calls, support, reviews, community, win/loss, search queries; cluster; extract 20–50-word audience vocabulary; never paraphrase testimonials.
- Frameworks: AIDA, PAS, BAB, 4Ps, FAB, StoryBrand. Scaffolds — edit ruthlessly.
- Headline formulas: "How to X without Y", "[Outcome] for [audience]", "The [N] [thing] that [outcome]", question-form, "[Time] to [outcome]", "Stop X. Start Y.", "[Category] without the [drawback]", Ogilvy fact-first.
- Hero: specific 6–12-word headline, 15–25-word subhead, verb+noun primary CTA, optional lower-commitment secondary, evidence, product screenshot (not stock).
- Value proposition: one sentence naming end-benefit, audience, alternative, unique mechanism.
- Page-type copy patterns: home (hero → trust → problem → solution → proof → objections → secondary CTA), pricing (3–4 tiers, anchored middle, "Everything in X, plus:", no dark patterns), feature (JTBD headline → mechanism → benefits with proof), about (user-facing mission → origin → team → values → hiring), blog (question-aligned title → deck → byline → TOC → TL;DR), 404/500 (acknowledge, redirect, no stack trace), legal/footer (plain-language summary on top), transactional email (subject states outcome, one action per email).
- Microcopy: verbs on buttons, visible labels on inputs, error messages that identify and fix, preserve user input on failure, bad→good example in every state.
- CTA psychology: name the outcome not the action; friction reducer beneath; unique primary per page; test button copy separately.
- Social proof: logos only with written permission, stats with source and denominator, testimonials with objection + person + outcome, case studies Before→Intervention→After→Quote.
- Nav and IA: nouns for destinations, verbs for actions, user vocabulary over org terminology.
- Notifications: toast (ephemeral) vs banner (persistent) vs modal (blocking); severity conveyed by icon + text, not color alone.
- Cookie consent: symmetric Accept/Reject/Manage; no pre-checked non-essential categories.
- Power words and specificity: sensory verbs over managerial; specific nouns over abstract; numbers over adjectives; avoid "revolutionary", "world-class", "cutting-edge" without evidence.
- Reading level: grade 7–9 marketing, 8–10 onboarding, 10–12 docs; sentences 14–18 words average, cap 25; paragraphs 2–4 sentences.
- Editing in 5 passes: truth → specificity → rhythm → voice → cut (remove 20%).
- Copy measurement: hypothesis first; test highest-traffic element; instrument downstream conversions; segment by source and device; qualitative 5-second test on the winner.
- SEO: same foundational rules apply to AI features; crawlable, text-forward, internally linked.
- Structured data: JSON-LD, visible-content parity, Rich Results Test, and page-type schema only where Google documents support.

## 03 — Visual design

- Rank content first; most-important element = visual anchor.
- Hierarchy tools in order: size, weight, color, spacing, position, contrast, motion. Combine 2–3.
- Safe defaults: near-black/near-white, saturated neutrals, optical alignment, 12-column grid, nested corners (outer = inner + padding), 16 px+ body, 60–75 ch line length.
- Spacing from a single scale (4, 8, 12, 16, 24, 32, 48, 64, 96 px).
- Typography: max two typefaces (prefer super-families), variable fonts, modular scale (1.125–1.333 ratios), fluid sizes via `clamp()`.
- Color: OKLCH for authoring, APCA as sanity check, 12-step scales (Radix model), always ship light and dark.
- Imagery: real, specific, brand-owned; one icon system, optically matched to text.
- Depth: consistent shadow scale; multi-layer shadows; never stack shadow + border + blur.
- Design styles: Swiss, minimalism, maximalism, brutalism, editorial — pick one primary.
- Layout archetypes: single column, F-pattern, Z-pattern, modular grid, bento, asymmetric, magazine, sidebar, single-page, card/feed, masonry, hub-and-spoke, fullbleed media. Pick one per page; mix at most one secondary.
- Hero patterns: centered text, split text/media, fullbleed media, asymmetric collage, editorial text-rich, interactive/live. Match to the brand's argument, not the tutorial default.
- Page sections are a vocabulary, not a checklist. Strong sites prune to 4–6; the AI default 9-section SaaS sequence reads as generated.
- Designing against AI defaults: reject indigo→violet gradients, Inter-only typography, 3-icon feature grids, generic 3D blob illustrations, glassmorphism on every card, identical 8/12/16 px rounded corners. See chapter 09 § AI design fingerprints.
- Browse [`inspiration-gallery.md`](inspiration-gallery.md) before generating from blank; pick 1–2 categories matching the brief, scan 5–10 examples, absorb structure not pixels.

## 04 — CSS and layout

- Units: `rem` for type and spacing, `px` for hairlines, `ch` for reading widths, `dvh/svh/lvh` for mobile viewport, `cqi/cqb` for container-scoped sizing.
- Never `vw` alone for font-size; use `clamp()`.
- Responsive preference order: natural flow → flex-wrap → grid `auto-fit` → `clamp()` → container queries → media queries.
- Canonical breakpoint-free grid: `grid-template-columns: repeat(auto-fit, minmax(min(20rem, 100%), 1fr))`.
- Container queries for component-level adaptation; name containers; `container-type: inline-size` is the default.
- `:has()` replaces many class-toggle JS patterns in current browsers; verify fallbacks if older browsers matter.
- `@layer reset, base, components, utilities` makes specificity predictable.
- Other modern features: native nesting, subgrid, `@scope`, View Transitions, scroll-driven animations, `@property`, `color-mix()`, `light-dark()`, `@starting-style`, `@function`, `if()` — use newly available and limited-availability features as progressive enhancement.
- Logical properties always (`margin-inline`, `padding-block`).
- Motion: ease-out for entry, ease-in for exit, springs for tactile, 150–300 ms, interruptible, animate `transform`/`opacity` only, respect reduced motion.
- `field-sizing: content` and anchor positioning are progressive enhancement; `popover` is broadly available in current browsers; `:user-invalid` supports post-interaction validation.

## 05 — Accessibility

- Semantic HTML first, ARIA only when HTML cannot express.
- Five ARIA rules: use native, do not redefine semantics, ensure keyboard, do not hide focusables, every interactive has accessible name.
- Keyboard: Tab/Enter/Space/Arrows/Escape/Home/End; no positive `tabindex`; only `0` and `-1`.
- Focus management: move into modals on open, return on close, move focus on route change, visible indicator always.
- `<dialog>` with `showModal()` handles focus trap natively.
- Skip link to `#main` as first focusable.
- Forms: visible `<label>`, `aria-describedby` for help/error, correct `input type`, `autocomplete` attributes, no disabled copy/paste.
- Motion respects `prefers-reduced-motion`; no auto-play carousels; limit parallax.
- Test: axe, Lighthouse, Accessibility Insights for automation; manual keyboard, screen reader (VoiceOver/NVDA/TalkBack), 200–400 % zoom, high-contrast mode.
- Prefer React Aria, Radix, Headless UI, shadcn/ui over hand-rolled menus, dialogs, comboboxes, date pickers.

## 06 — Performance

- Core Web Vitals thresholds: LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1 (at the 75th percentile of real users).
- LCP: identify the likely hero image early, use `fetchpriority="high"`, preload when discovery would otherwise be late, use AVIF/WebP, defer non-critical CSS/JS, self-host fonts, server-render above-the-fold content.
- INP: keep tasks < 50 ms, use `scheduler.yield()`, debounce handlers, avoid forced sync layout, move heavy work to Web Workers, defer third-party scripts, hydrate selectively.
- CLS: always set `width`/`height`, reserve space for ads and embeds, use matched-metric font fallbacks or `font-display: optional`, never inject content above existing content.
- Images: resize to display size, `srcset`/`sizes`, modern formats, `loading="lazy"` below fold, image CDN.
- Fonts: WOFF2 only, subset, preload the primary, metric-matched fallbacks.
- Third parties are the main INP offender; facade-load video/map/chat embeds.
- Critical path: inline critical CSS, small initial HTML, `defer` scripts, `preconnect`/`dns-prefetch`, Brotli + HTTP/2 or HTTP/3.
- Caching: immutable fingerprinted assets for a year; HTML with `stale-while-revalidate`.
- bfcache: never use `unload`, observe `pageshow` / `pagehide`, and restore transient state correctly on history navigation.
- Bundle discipline: audit monthly, tree-shake, replace heavy libs with platform or lighter alternatives.
- Rendering: SSG for static, ISR for mostly-static, SSR for per-request, edge for latency-sensitive.
- Monitoring: `web-vitals` RUM + CrUX + Lighthouse CI + synthetic; enforce budgets in CI.
- Progressive enhancement: HTML-first; page works before JS.

## 07 — JavaScript and HTML

- Use native: `<dialog>`, `<details>`, `popover` attribute, `inert`, `content-visibility`, `IntersectionObserver`, `ResizeObserver`, `AbortController`.
- ES2025 (approved June 2025) adds: iterator helpers, Set methods, `RegExp.escape`, regex flag modifiers, duplicate named capture groups, `Promise.try`, import attributes (`with { type: "json" }`, `with { type: "css" }`), JSON modules, `Float16Array`.
- Temporal — Stage 4/ES2026; Firefox 139+, Chrome/Edge 144+; Safari not shipped (Apr 2026) — use `temporal-polyfill`. Prefer Temporal over `Date`.
- Signals (TC39 Stage 1): cross-framework reactive primitive; use `signal-polyfill` today.
- Web components: Shadow DOM for encapsulated design-system widgets; Light DOM for HTML-with-superpowers; form-associated with `attachInternals()`; declarative Shadow DOM via `<template shadowrootmode="open">`; Lit for non-trivial scale.
- Runtimes: Node for default, Bun for speed and simple tooling, Deno for security and single-binary distribution; WinterCG Web-standard APIs (`fetch`, `Request`, `Response`, streams) are portable across all three.
- Security: HTTPS, strict CSP, security headers, no `eval`, sanitize on the server, `HttpOnly` cookies.
- TypeScript: strict mode, prefer inference, discriminated unions, types from schemas.
- Dates: UTC ISO 8601 stored, convert at render, Temporal with polyfill.

## 08 — Stack and architecture

- Framework choice: Astro for content; Next.js for React apps; SvelteKit for smallest bundles; Remix/React Router v7 for web-standards-first; no framework for static micro-sites.
- Project-type recipes: marketing/blog (Astro + CMS + Vercel), small SaaS (Next.js + Postgres + Clerk + Stripe + Vercel), dashboard (Next.js + TanStack + Postgres), real-time (Liveblocks/PartyKit + Yjs + Fly.io), e-commerce (Shopify headless or Medusa), enterprise (SSO + RLS + audit), AI-augmented (FastAPI/Vercel AI SDK + pgvector).
- Rendering per route: SSG for marketing/docs, ISR for semi-dynamic, SSR for personalized and authenticated, streaming SSR for mixed-latency pages, CSR only behind a login with no SEO, edge for global personalization, islands for mostly-static sites with interactive widgets, resumability (Qwik) for instant interactivity, React Server Components for server-heavy trees with client islands. Mix strategies; never SPA a public marketing site.
- Backend and API style: Next.js route handlers or Server Actions for same-team React; tRPC for TypeScript monorepos; REST + OpenAPI for multi-consumer and partner APIs (define the contract first, codegen clients, version explicitly); GraphQL only when the graph is genuine and persisted queries, cost limits, and field-level authorization are in place; gRPC or Connect for service-to-service; WebSockets for bidirectional real-time, SSE for server push; webhooks with signed payloads, idempotency keys, and fast 2xx.
- Database default: PostgreSQL. SQLite/Turso for embedded; ClickHouse/TimescaleDB for analytical; Redis for cache/sessions/rate limiting; pgvector before specialized vector stores.
- CMS choice: Markdown for docs; Sanity or Payload for small editorial teams; Storyblok for visual editing; Contentful for enterprise governance; Directus over an existing SQL database.
- Auth provider: Clerk (fastest), Auth.js/Better Auth (self-hosted), WorkOS/Auth0/Okta/Entra ID (enterprise SSO). Never hand-roll.
- Hosting: Vercel/Netlify/Cloudflare for stateless web; Fly.io/Railway/ECS for containers and long-lived processes; AWS/GCP/Azure via IaC for heavy custom infra.
- Styling: Tailwind v4 (CSS-first `@theme`, OKLCH palette) is the default; CSS Modules or Vanilla Extract as alternatives; plain CSS with `@layer` is viable.
- Design tokens in tiers: primitive → semantic → component. Name by intent, not value.
- shadcn/ui + Radix + CVA for React component defaults. React Aria for rigorous a11y.
- Composition over configuration; co-locate component assets (template, styles, tests, types).
- Naming: PascalCase components, camelCase utilities, kebab-case files and classes, `visible` not `isVisible`, `set*` setters.
- State scope: local state first, then lift, then small store (Zustand/Jotai/Nanostores); server state via TanStack Query/SWR; forms via React Hook Form.
- Deployment: CI runs type/lint/a11y/LHCI/bundle checks; preview deploys per PR; canary rollouts; RUM from day one.
- Scaling ladder: single server → read replica → Redis → CDN → background queues → worker containers → search index → cache-ahead → partition/shard → split the monolith. Only when the current step actually hurts.
- Maintainability: boring technology, one language in the hot path, no bleeding edge in critical code, every dependency is a liability, platform primitives over libraries.
- Boring-tech budget: novelty on 1–2 components with real advantage; rest conventional.
- Docs: `README`, `ARCHITECTURE`, `CONTRIBUTING`, `DESIGN_TOKENS` kept up to date.

## 09 — Anti-patterns and process

- Hero: vague verbs, generic stock, double primary CTAs, autoplay carousels.
- Typography: body under 16 px, `vw`-only type, three unrelated typefaces.
- Color: pure `#000` on pure `#FFF`, color-only meaning, blind-inverted dark mode.
- Motion: full-page parallax, scroll-jacking, animating `width`/`height`.
- Layout: `100vh` on mobile (use `100dvh`), hard-coded widths, missing `min-width: 0` on flex children, `<pre>`/`<code>` without `overflow-x: auto` (commands overflow), `overflow: hidden` sized to cap-height clips descenders (`g`, `y`, `p`, `q`).
- HTML: `<div onClick>`, `<a href="#">` as button, missing `alt`.
- CSS: `!important` outside utility layers, z-index spiral, `position: absolute` for layout.
- JS: `==`, silent catches, `innerHTML` with user input, tokens in `localStorage`.
- React: misused `useEffect`, unstable list keys, new function references as props.
- AI pitfalls: hallucinated imports, hardcoded pixels, missing `width`/`height`, ignoring reduced motion, defensive ARIA, reimplementing modals from scratch.
- AI design fingerprints to refuse: indigo/violet gradient hero, blurred radial blobs behind centered text, Inter-only type, 3-icon feature grid, identical rounded-card+soft-shadow everywhere, lucide outline icon on every block, glassmorphism by default, 9-section SaaS template ordering, stock-photo team smiling at laptops, generated isometric 3D illustrations, announcement pill / eyebrow badge above the hero headline, category/type tag pills beneath feature cards.
- Pre-merge checklist covers correctness, design, a11y, performance, copy, and code quality.
- Process: brief → content → wireframe → tokens → components → pages → a11y/perf → QA → staged launch → 30/90-day review.
- Design review weekly, 15–30 minutes, seven lenses: hierarchy, consistency, brand, feel, copy, a11y, performance.

## 10 — Resources

- Standards: WCAG 2.2, ARIA APG, MDN, HTML Spec, Can I Use, Baseline.
- Design: Refactoring UI, Material 3, Apple HIG, Every Layout, Anthony Hobday, Laws of UX, NN/g.
- Copy: Mailchimp, Polaris, GDS, StoryBrand.
- Type: Utopia, Type Scale, Modular Scale, Fontsource, Fontaine, Wakamai Fondue.
- Color: `oklch.com`, Radix Colors, Huemint, Coolors, Realtime Colors, Leonardo, APCA, Contrast Tools.
- Motion: GSAP, Motion, Rauno, Emil Kowalski, cubic-bezier.com, Josh W. Comeau.
- A11y: axe, Accessibility Insights, Pa11y, WAVE, WebAIM surveys, NVDA, VoiceOver.
- Performance: PageSpeed Insights, CrUX, `web-vitals`, WebPageTest, Lighthouse CI, Bundlephobia, Squoosh, Sharp.
- Component primitives: Radix, React Aria, Headless UI, shadcn/ui, Chakra, Ark.
- Frameworks: Next.js, Astro, SvelteKit, React Router v7, Nuxt, SolidStart, Qwik.
- Runtimes: Node.js, Bun, Deno.
- Study: Stripe, Linear, Vercel, Figma, Apple, Ramp, Mercury, Raycast, Superhuman, GitHub Primer, Awwwards.

## 11 — Browser compatibility

- Global market share (StatCounter, February 2026): Chrome 68.98%, Safari 16.39%, Edge 5.46%, Firefox 2.29%, Samsung Internet 2.01%, Opera 1.78%. Regional skew: Safari higher in North America and Oceania; Samsung Internet meaningful in Africa and parts of Asia.
- Pick a support matrix before writing code: analytics-driven, Baseline-driven, or contractual. Write it into the project README.
- Baseline tiers: Widely Available (ship unguarded, interoperable 30+ months), Newly Available (feature-detect, interoperable across core browsers now), Limited Availability (experiment, polyfill).
- Browserslist v4.26+ accepts `baseline widely available`, `baseline newly available`, `baseline 2024` (or any year), and `baseline widely available with downstream` queries. Share one `browserslist` across Autoprefixer, Babel, PostCSS, ESLint, Stylelint.
- Feature-detect with `@supports`, `CSS.supports()`, and object checks; never user-agent sniff.
- Prefer ponyfills over global polyfills; load only after detection fails.
- Now Baseline Newly Available (do not overlook): `@scope`, invoker commands (`command`/`commandfor`), `scrollbar-color`, `scrollend`, Navigation API, CSS `shape()`, Trusted Types, Zstandard, WebTransport, `view-transition-class`, `rcap`/`rch`/`rex`/`ric` units, `Iterator.concat()`, `Map.getOrInsert()`.
- Still Limited Availability (April 2026): anchor positioning, `field-sizing`, scroll-driven animations, `interpolate-size` / `calc-size()`, container style queries, CSS `@function`/`if()`, Temporal (Safari-only gap), Signals, scoped `CustomElementRegistry`, `URLPattern`, WebGPU.
- Test locally on latest Chrome + Safari on macOS; on real iOS and Android devices; against the matrix in CI via Playwright, BrowserStack, or Sauce.
- CI enforcement: `eslint-plugin-compat` v7.x, `stylelint-no-unsupported-browser-features` v8.x (or `stylelint-browser-compat` / `stylelint-plugin-use-baseline` alternatives), shared `browserslist` config, RUM segmented by browser.
- Interop 2026 is tracked at `wpt.fyi/interop-2026`.

## 12 — Testing

- The testing trophy: static analysis → integration (largest) → unit → E2E → visual regression.
- Vitest replaces Jest for Vite-based projects: 4–10× faster, native TypeScript/ESM, Jest-compatible API.
- MSW intercepts at the network layer — components call real `fetch`, tests exercise actual data-fetching code.
- Integration tests: render with real hooks and MSW-mocked API; query by role, label, and text; no snapshot tests of HTML.
- Playwright for E2E: Chromium/Firefox/WebKit in parallel, auto-waiting, trace viewer, codegen; prefer role selectors over `data-testid`.
- Visual regression: `toHaveScreenshot` with `animations: 'disabled'`; Docker for consistent cross-OS rendering; mask dynamic content.
- Accessibility automation: axe-core in integration tests; `@axe-core/playwright` in E2E; zero-violation policy in CI.
- Flaky tests are worse than no tests. Auto-waiting over `waitForTimeout`. Run `--repeat-each 10` before committing.
- CI: typecheck → lint → unit+integration → Playwright E2E → a11y. Fail the build on any failure.

## 13 — Internationalization

- i18n (architectural) vs l10n (translation/adaptation). Retrofitting costs 3–5× more.
- Intl API: `DateTimeFormat`, `NumberFormat`, `RelativeTimeFormat`, `ListFormat`, `PluralRules`, `Collator`, `Segmenter` — all built-in, zero bundle cost.
- ICU MessageFormat for pluralization and interpolation. Never concatenate translated fragments with variables.
- Locale detection order: saved preference → URL segment → `navigator.language` → default.
- URL structure: subdirectories (`/de/`) are the pragmatic default.
- RTL: use logical properties (`margin-inline-start` not `margin-left`) everywhere; `dir="rtl"` on `<html>`; mirror directional icons.
- Text expansion: German ~30% longer than English. Use `min-width`, not `width`, on buttons. Test with longest locale.
- hreflang: reciprocal, self-referencing, always include `x-default`. For 1000+ pages, use XML sitemap not HTML.
- Language switcher: label in the target language ("Deutsch" not "German"); use language names not flags; persist choice.
- Performance: lazy-load the active locale; split by namespace for large apps.
- Pseudo-localization in dev to catch hardcoded strings and layout overflow early.
- WCAG 1.3.2: `lang` on `<html>` is Level A. Screen readers use it for speech synthesizer selection.

## 14 — Security

- Security headers set on every response: CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy.
- CSP: start with `Content-Security-Policy-Report-Only` to discover violations; use nonces for inline scripts; avoid `'unsafe-inline'` for `script-src`.
- HSTS: `max-age=63072000; includeSubDomains; preload`. Start with `max-age=300` during testing.
- OWASP Top 10 (2025 RC1): Broken Access Control (SSRF merged in), Security Misconfiguration, Software Supply Chain Failures, Cryptographic Failures, Injection, Insecure Design, Authentication Failures, Software or Data Integrity Failures, Security Logging & Alerting Failures, Mishandling of Exceptional Conditions.
- XSS: use framework native rendering; DOMPurify for user HTML; never `eval()` with untrusted input; consider Trusted Types.
- CSRF: `SameSite: Lax` cookies as the primary defence; CSRF tokens for state-changing forms.
- Input validation: client-side is UX; server-side is security. Use Zod schemas derived from a single source of truth.
- Auth: `HttpOnly`, `Secure`, `SameSite` cookies; Argon2id or bcrypt (work factor 12); prefer managed auth over custom.
- CORS: whitelist specific origins; never blindly reflect the incoming `Origin` header.
- File uploads: validate MIME type from actual bytes; sanitize filenames; serve uploads from a separate origin.
- Dependencies: `npm audit` in CI; Dependabot or Renovate for automated updates; quarterly manual audit.
- Secrets: never committed to source; rotated on schedule; rotate immediately if accidentally committed.

## 15 — Discovery and communication

- Ask when the answer changes the output; default silently when a strong answer exists in this guide.
- Minimum brief before building: audience, primary action, differentiator, target feel, success metric, deadline and scope, existing brand assets, content status, technical constraints, compliance, i18n, expected scale.
- Stage-gated questions: brief → voice → sitemap → copy → tokens → components → pages → a11y/perf passes → launch. Do not ask ahead of the stage.
- Always default, never ask: body type and spacing scales, focus styles, WCAG 2.2 AA thresholds, reset CSS, `prefers-reduced-motion`, image defaults, lint/format, security-header baseline.
- Always ask before committing: framework, hosting, CMS, database, auth provider, payments, analytics, email provider, brand archetype, any irreversible choice (domain, schema, URL slugs).
- Surface trade-offs, state assumptions, preserve user copy verbatim, flag non-negotiable violations before following a user override.
- Red-flag phrases to clarify: "make it pop", "like [brand]", "SEO-optimized", "responsive", "simple" + long feature list, "enterprise" for small audience, "AI-powered" with no use case, "just the basics", "like before but better".
- Offer explicit scope trade-offs when features exceed deadline; never half-ship everything silently.
- Confirm non-frontend inputs when relevant: data model, user roles, content workflow, payments, email, uploads, search, notifications, background work, observability, legal, handoff.
- Do not ask questions answered by the brief or by this guide; do not ask twice.
- Close the loop: produce a post-launch note listing what shipped, what was deferred, assumptions made, and the 30/90-day review against the success metric.

## 16 — Code style and quality

- Formatting is mechanical: Prettier 3 with `trailingComma: "all"`, `printWidth: 100`, `singleQuote: false`, `semi: true`; Biome acceptable as a unified formatter+linter when the stack is JS/TS-only. Never debate layout in review.
- EditorConfig mirrors the formatter; `.editorconfig` is the baseline: `indent_size = 2`, `end_of_line = lf`, `insert_final_newline = true`, `trim_trailing_whitespace = true`.
- Lint with ESLint 9+ flat config and `typescript-eslint` unified package. Use `@eslint/js`, `typescript-eslint`, `eslint-plugin-import-x`, `eslint-plugin-jsx-a11y`, `eslint-plugin-unicorn`, `eslint-plugin-promise`. Stylelint with `stylelint-config-standard` and `stylelint-config-clean-order` for CSS.
- `tsconfig` baseline: `strict`, `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, `verbatimModuleSyntax`, `isolatedModules`, `noPropertyAccessFromIndexSignature`.
- Repository hygiene: `.gitignore` (build output, `.env`, `.DS_Store`), `.gitattributes` (`* text=auto eol=lf`), `.editorconfig`, `.nvmrc` or `.tool-versions`, `.vscode/` with workspace settings and recommended extensions, `.github/PULL_REQUEST_TEMPLATE.md`, `.github/CODEOWNERS`.
- Naming: PascalCase for classes/components/types; camelCase for functions, methods, variables; UPPER_SNAKE_CASE for constants; kebab-case for file names (except framework conventions that dictate PascalCase components); CSS classes kebab-case; drop the `is` prefix on booleans.
- Imports: group 1 node builtins, 2 third-party, 3 internal path aliases (`@/`), 4 relative; alphabetize within groups; one blank line between groups; named exports by default; no `export *` barrels; no deep relative imports (`../../`).
- Comments: explain "why," never "what the code does." `TODO:` and `FIXME:` carry author initials and a ticket reference; `HACK:` documents the limitation. No AI narration in production code.
- Error handling: never silently swallow; `throw new Error("operation: context")`; preserve cause (`new Error(msg, { cause })`); catch at boundaries; use `Result<T, E>` (neverthrow) for expected failures in hot paths; treat `unknown` as the default catch type.
- Logging: `pino` on Node, `tslog` elsewhere; JSON single-line output; required fields `service`, `env`, `version`, `request_id`; redact secrets at the logger level; levels mean severity, not verbosity; never log user PII by default.
- Dependency management: pnpm default, exact versions, commit the lockfile, `packageManager` pinned, Renovate for automation, audit in CI, document `overrides` in `ARCHITECTURE.md`, evaluate every new dependency.
- Build toolchain: Vite for apps (Rolldown when stable); tsdown or tsup for libraries; ESM-only new code; emit source maps; lazy-load route chunks.
- Git: Conventional Commits enforced by `commitlint`; `main` is the only long-lived branch; squash-merge default; Husky or Lefthook for hooks; `lint-staged` runs formatter and linter on changed files; `pre-push` runs type check and fast tests; signed commits on production repos.
- Release: `changesets` for monorepos, `semantic-release` for single packages. Tie release to Conventional Commit history.
- Readability limits: max ~80 lines per function before extraction; max ~400 lines per file; cyclomatic complexity warning at 10.
- CI enforcement: type check, lint, format-check, test, bundle-size budget, dependency audit, license check. Fail fast; fail loud.

## The shortest review

Does change make site easier to use, understand, maintain, faster? If not, don't ship.
