# Summary

A condensed version of the full guide. Load this file alone when context is tight; load the full chapters for depth.

## Premise

A website serves a specific person, to accomplish a specific goal, in a specific brand voice. Every choice — color, word, spacing, motion — is evaluated against those three variables. Craft is the input; beauty is the output.

## Non-negotiables

- Every `<img>` has `width`, `height`, and meaningful `alt`.
- The LCP image is preloaded with `fetchpriority="high"` and never lazy-loaded.
- Body text ≥ 16 px, line-height 1.5, line length 60–75 characters.
- WCAG 2.2 AA contrast: 4.5:1 body text, 3:1 large text, 3:1 UI and focus rings.
- Visible `:focus-visible` on every focusable element.
- `prefers-reduced-motion` is honored.
- Native HTML first: `<button>`, `<dialog>`, `<details>`, `<popover>` before custom.
- Fonts use `font-display: swap` with matched-metric fallbacks or `font-display: optional`.
- No positive `tabindex`.
- Two-space indent, kebab-case files, `visible` not `isVisible`, logical properties, `.DS_Store` in `.gitignore`.

## 01 — Philosophy and psychology

- Answer four questions before designing: who is this for, what is the primary action, why trust this, what feeling.
- Laws of UX to apply: Jakob (conventions), Hick (fewer choices), Fitts (target size), Miller (7 ± 2), Doherty (< 400 ms feels fast), Peak–End (invest in hero and final states), Zeigarnik (show progress).
- Nielsen's 10 heuristics; Gestalt (proximity, similarity, continuity, closure).
- Cialdini's 7 principles: reciprocity, commitment, social proof, authority, liking, scarcity, unity.
- Don Norman's three levels: visceral, behavioral, reflective.
- 5-second test: what is this, who is it for, what can you do.

## 02 — Brand and copywriting

- Document brand in writing: audience, offer, reason-to-exist, personality, archetype, voice-with-but-not modifiers.
- Pick one of Jung's 12 archetypes; mixing more than two produces incoherence.
- Voice is fixed; tone adapts per context (onboarding, error, celebration, destructive confirmation).
- Frameworks: AIDA, PAS, BAB, 4Ps, FAB, StoryBrand. Frameworks are scaffolds — edit ruthlessly.
- Hero pattern: specific 6–12-word headline, one-sentence proof, verb+noun primary CTA, evidence, product screenshot (not stock).
- Microcopy: verbs on buttons, visible labels on inputs, error messages that identify and fix, preserve user input on failure.
- SEO for AI era: E-E-A-T, single-question paragraphs, JSON-LD, crawlable server-rendered content.

## 03 — Visual design

- Rank content first; most-important element becomes visual anchor; others defer.
- Hierarchy tools in order: size, weight, color, spacing, position, contrast, motion. Combine 2–3.
- Safe defaults: near-black/near-white, saturated neutrals, optical alignment, 12-column grid, nested corners (outer = inner + padding), 16 px+ body, 60–75 ch line length.
- Spacing from a single scale (4, 8, 12, 16, 24, 32, 48, 64, 96 px).
- Typography: max two typefaces (prefer super-families), variable fonts, modular scale (1.125–1.333 ratios), fluid sizes via `clamp()`.
- Color: OKLCH for authoring, APCA as sanity check, 12-step scales (Radix model), always ship light and dark.
- Imagery: real, specific, brand-owned; one icon system, optically matched to text.
- Depth: consistent shadow scale; multi-layer shadows; never stack shadow + border + blur.
- Design styles: Swiss, minimalism, maximalism, brutalism, editorial — pick one primary.

## 04 — CSS and layout

- Units: `rem` for type and spacing, `px` for hairlines, `ch` for reading widths, `dvh/svh/lvh` for mobile viewport, `cqi/cqb` for container-scoped sizing.
- Never `vw` alone for font-size; use `clamp()`.
- Responsive preference order: natural flow → flex-wrap → grid `auto-fit` → `clamp()` → container queries → media queries.
- Canonical breakpoint-free grid: `grid-template-columns: repeat(auto-fit, minmax(min(20rem, 100%), 1fr))`.
- Container queries for component-level adaptation; name containers; `container-type: inline-size` is the default.
- `:has()` replaces many class-toggle JS patterns.
- `@layer reset, base, components, utilities` makes specificity predictable.
- Other modern features: native nesting, subgrid, `@scope`, View Transitions, scroll-driven animations, `@property`, `color-mix()`, `light-dark()`, `@starting-style`, `@function`, `if()`.
- Logical properties always (`margin-inline`, `padding-block`).
- Motion: ease-out for entry, ease-in for exit, springs for tactile, 150–300 ms, interruptible, animate `transform`/`opacity` only, respect reduced motion.
- `field-sizing: content` for auto-growing inputs; `popover` + anchor positioning for tooltips/menus; `:user-invalid` for post-interaction validation.

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
- LCP: preload the hero image with `fetchpriority="high"`, use AVIF/WebP, defer non-critical CSS/JS, self-host fonts, server-render above-the-fold content.
- INP: keep tasks < 50 ms, use `scheduler.yield()`, debounce handlers, avoid forced sync layout, move heavy work to Web Workers, defer third-party scripts, hydrate selectively.
- CLS: always set `width`/`height`, reserve space for ads and embeds, use matched-metric font fallbacks or `font-display: optional`, never inject content above existing content.
- Images: resize to display size, `srcset`/`sizes`, modern formats, `loading="lazy"` below fold, image CDN.
- Fonts: WOFF2 only, subset, preload the primary, metric-matched fallbacks.
- Third parties are the main INP offender; facade-load video/map/chat embeds.
- Critical path: inline critical CSS, small initial HTML, `defer` scripts, `preconnect`/`dns-prefetch`, Brotli + HTTP/2 or HTTP/3.
- Caching: immutable fingerprinted assets for a year; HTML with `stale-while-revalidate`.
- Bundle discipline: audit monthly, tree-shake, replace heavy libs with platform or lighter alternatives.
- Rendering: SSG for static, ISR for mostly-static, SSR for per-request, edge for latency-sensitive.
- Monitoring: `web-vitals` RUM + CrUX + Lighthouse CI + synthetic; enforce budgets in CI.
- Progressive enhancement — HTML-first so the page works before JS arrives.

## 07 — JavaScript and HTML

- Use native: `<dialog>`, `<details>`, `<popover>`, `inert`, `content-visibility`, `IntersectionObserver`, `ResizeObserver`, `AbortController`.
- ES2025 (approved June 2025) adds: iterator helpers, Set methods, `RegExp.escape`, regex flag modifiers, duplicate named capture groups, `Promise.try`, import attributes (`with { type: "json" }`, `with { type: "css" }`), JSON modules, `Float16Array`.
- Temporal — Stage 4, ships in ES2026; native in Firefox 139+ and Chrome/Edge 144+; Safari not shipped as of April 2026, so use `temporal-polyfill` for cross-browser support. Any new code should prefer Temporal over `Date`.
- Signals (TC39 Stage 1): cross-framework reactive primitive; use `signal-polyfill` today.
- Web components: Shadow DOM for encapsulated design-system widgets; Light DOM for HTML-with-superpowers; form-associated with `attachInternals()`; declarative Shadow DOM via `<template shadowrootmode="open">`; Lit for non-trivial scale.
- Runtimes: Node for default, Bun for speed and simple tooling, Deno for security and single-binary distribution; WinterCG Web-standard APIs (`fetch`, `Request`, `Response`, streams) are portable across all three.
- Security: HTTPS, strict CSP, security headers, no `eval`, sanitize on the server, `HttpOnly` cookies.
- TypeScript: strict mode, prefer inference, discriminated unions, types from schemas.
- Dates: UTC ISO 8601 stored, convert at render, Temporal with polyfill.

## 08 — Stack and architecture

- Framework choice: Astro for content; Next.js for React apps; SvelteKit for smallest bundles; Remix/React Router v7 for web-standards-first; no framework for static micro-sites.
- Styling: Tailwind v4 (CSS-first `@theme`, OKLCH palette) is the default; CSS Modules or Vanilla Extract as alternatives; plain CSS with `@layer` is viable.
- Design tokens in tiers: primitive → semantic → component. Name by intent, not value.
- shadcn/ui + Radix + CVA for React component defaults. React Aria for rigorous a11y.
- Composition over configuration; co-locate component assets (template, styles, tests, types).
- Naming: PascalCase components, camelCase utilities, kebab-case files and classes, `visible` not `isVisible`, `set*` setters.
- State scope: local state first, then lift, then small store (Zustand/Jotai/Nanostores); server state via TanStack Query/SWR; forms via React Hook Form.
- Auth: framework- or platform-provided; session in `HttpOnly` cookies; Argon2id for passwords.
- Deployment: CI runs type/lint/a11y/LHCI/bundle checks; preview deploys per PR; canary rollouts; RUM from day one.
- Docs: `README`, `ARCHITECTURE`, `CONTRIBUTING`, `DESIGN_TOKENS` kept up to date.

## 09 — Anti-patterns and process

- Hero: vague verbs, generic stock, double primary CTAs, autoplay carousels.
- Typography: body under 16 px, `vw`-only type, three unrelated typefaces.
- Color: pure `#000` on pure `#FFF`, color-only meaning, blind-inverted dark mode.
- Motion: full-page parallax, scroll-jacking, animating `width`/`height`.
- Layout: `100vh` on mobile (use `100dvh`), hard-coded widths, missing `min-width: 0` on flex children.
- HTML: `<div onClick>`, `<a href="#">` as button, missing `alt`.
- CSS: `!important` outside utility layers, z-index spiral, `position: absolute` for layout.
- JS: `==`, silent catches, `innerHTML` with user input, tokens in `localStorage`.
- React: misused `useEffect`, unstable list keys, new function references as props.
- AI pitfalls: hallucinated imports, hardcoded pixels, missing `width`/`height`, ignoring reduced motion, defensive ARIA, reimplementing modals from scratch.
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
- i18n (architectural) vs l10n (translation/adaptation). Retrofitting costs 3–5× more than building from the start.
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
- OWASP Top 10 (2026): Broken Access Control, Cryptographic Failures, Injection, Insecure Design, Security Misconfiguration, Vulnerable Components, Authentication Failures, Data Integrity Failures, Logging Failures, SSRF.
- XSS: use framework native rendering; DOMPurify for user HTML; never `eval()` with untrusted input; consider Trusted Types.
- CSRF: `SameSite: Lax` cookies as the primary defence; CSRF tokens for state-changing forms.
- Input validation: client-side is UX; server-side is security. Use Zod schemas derived from a single source of truth.
- Auth: `HttpOnly`, `Secure`, `SameSite` cookies; Argon2id or bcrypt (work factor 12); prefer managed auth over custom.
- CORS: whitelist specific origins; never blindly reflect the incoming `Origin` header.
- File uploads: validate MIME type from actual bytes; sanitize filenames; serve uploads from a separate origin.
- Dependencies: `npm audit` in CI; Dependabot or Renovate for automated updates; quarterly manual audit.
- Secrets: never committed to source; rotated on schedule; rotate immediately if accidentally committed.

## The shortest review

One question covers most cases: does the change make the site easier to use, easier to understand, easier to maintain, and faster? If not, it should not ship.
