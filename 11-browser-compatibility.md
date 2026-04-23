# Browser Compatibility

Ship new platform features without shipping broken pages. The web is a moving target; this chapter documents how to decide a support target, how to detect features, how to degrade gracefully, and how to test the result.

## The 2026 browser landscape

Global market share, StatCounter Global Stats, February 2026, all platforms combined:

- Chrome — 68.98%
- Safari — 16.39%
- Edge — 5.46%
- Firefox — 2.29%
- Samsung Internet — 2.01%
- Opera — 1.78%

Split by platform:

- Desktop — Chrome 73.39%, Edge 10.99%, Safari 6.14%, Firefox 4.19%, Opera 2.21%, Brave 1.36%.
- Mobile — Chrome 65.55%, Safari 25.33%, Samsung Internet 3.91%, Opera 1.42%, UC Browser 1.04%, Firefox 0.63%.

These numbers shift regionally. Safari usage is substantially higher in North America (~30%) and Oceania; Samsung Internet is meaningful in Africa and parts of Asia; Opera Mini still has significant reach in emerging markets. Always sanity-check worldwide averages against the site's own analytics or CrUX data.

All four core engines are now collaborating on interoperability through Interop 2026, announced in February 2026 by Apple, Google, Igalia, Microsoft, and Mozilla. Progress is tracked at `wpt.fyi/interop-2026`.

## Decide the support matrix before writing code

Every project needs a written answer to "which browsers and versions must this work in?" Pick one of three defaults:

1. Analytics-driven. Pull the last 30–90 days of browser and version data from analytics (GA4, Plausible, Fathom, server logs) or CrUX for the origin. Support every browser that covers ≥ 0.5% of users, or whatever threshold the business agrees to. Recheck quarterly. The `baseline-browser-mapping` npm module joins analytics rows to Baseline tiers automatically; the Google Analytics Baseline Checker is a drop-in UI for GA4 exports.
2. Baseline-driven. Adopt the W3C Baseline shipped via `web-features`. "Widely Available" (interoperable across Chrome, Firefox, Safari, and Edge for at least 30 months) is the pragmatic threshold for shipping a feature without guards. "Newly Available" (interoperable across the latest versions of all four, but less than 30 months old) needs guards or feature detection. Fixed targets like "Baseline 2024" are also available for projects that need deterministic support sets.
3. Contractual. Some clients mandate specific browsers (IE-era enterprise, government kiosks, regulated industries). Treat the contract as the matrix and document the cost of every legacy version.

Write the matrix into `README.md` or `ARCHITECTURE.md` so every contributor knows the target.

A sensible default for a 2026 greenfield site with no contractual constraints:

- Chrome, Edge, Firefox, Safari — latest two stable versions, and anything shipping within the preceding 12 months.
- Chrome for Android and Safari on iOS — current-1 and current.
- Samsung Internet — current-1 and current.
- Everything else — best-effort, progressive enhancement; no guarantees.

This default is equivalent to the Browserslist `defaults` query (`> 0.5%, last 2 versions, Firefox ESR, not dead`) and to Baseline Widely Available for anything shipping two-and-a-half years ago or earlier.

Do not silently support browsers that are not in the matrix, and do not silently drop browsers that are in it.

## Baseline: the current shared language

Baseline is maintained by the WebDX Community Group (W3C) and backed by Apple, Google, Microsoft, and Mozilla. The data source is the `web-features` npm package at `github.com/web-platform-dx/web-features`. Baseline classifies features into three states:

- Limited availability — does not yet work across the four major engines. Treat as experimental; feature-detect and polyfill.
- Newly available — interoperable across Chrome (desktop and Android), Firefox (desktop and Android), Safari (macOS and iOS), and Edge at their latest stable versions. Safe to adopt for users on current browsers; older versions still need a fallback.
- Widely available — all four engines have supported it for 30+ months. Safe to ship without guards for any realistic audience.

Baseline also publishes fixed yearly snapshots (`Baseline 2022`, `Baseline 2023`, etc.) that do not evolve over time — useful when predictability matters more than chasing the latest features.

Reference Baseline badges directly from MDN, Can I Use, and `webstatus.dev` when justifying a feature in a PR. "Widely available" removes most of the need for `@supports` guards; "newly available" still requires them.

### Baseline-aware tooling

As of April 2026, Baseline is integrated into the tooling most projects already use:

- Browserslist (v4.26+) accepts `baseline widely available`, `baseline newly available`, `baseline 2024` (or any year from 2015 onward), and `baseline widely available with downstream` for Chromium-based browsers outside the core set.
- ESLint has official Baseline linting for CSS via the web.dev integration.
- Stylelint plugins: `stylelint-no-unsupported-browser-features` (current 8.1.x) uses `doiuse` + Browserslist; `stylelint-browser-compat` uses MDN data; `stylelint-plugin-use-baseline` enforces Baseline rules directly without Browserslist.
- VS Code ships a built-in Baseline support indicator for CSS properties.
- Chrome DevTools displays Baseline status in the Elements panel for CSS properties.
- Netlify has an official Baseline extension that reports on the audience's support coverage.
- `eslint-plugin-compat` (v7.x) lints browser-compatibility of Web APIs and ES APIs against a Browserslist target.

### Example Browserslist configurations

```json path=null start=null
{
  "browserslist": [
    "baseline widely available"
  ]
}
```

Ship features only once they have been interoperable for 30 months. Safest threshold; no feature detection needed for any feature in this tier.

```json path=null start=null
{
  "browserslist": [
    "baseline newly available with downstream"
  ]
}
```

Target anything currently interoperable across the core four engines plus Chromium-based downstream browsers (Samsung Internet, Opera, etc.). Any non-Baseline feature still needs a guard.

```json path=null start=null
{
  "browserslist": [
    "defaults and fully supports es6-module",
    "maintained node versions"
  ]
}
```

The recommended `browsersl.ist` default for general-purpose web apps.

```json path=null start=null
{
  "browserslist": [
    "> 0.5% in my stats",
    "last 2 versions",
    "not dead"
  ]
}
```

Analytics-driven: combine real-user data from `browserslist-ga`, `browserslist-plausible`, or a custom `browserslist-stats.json` file with a minimum floor of the last two versions of each browser.

## Progressive enhancement patterns

The default architecture: build with platform primitives first, layer modern features on top, and make sure the page works when any layer fails.

### HTML-level fallbacks

- `<picture>` with `<source type="image/avif">`, `<source type="image/webp">`, and a final `<img>` fallback. The browser picks the first format it understands.
- Form submission and link navigation work server-side without JavaScript. Intercept only when necessary.
- Native elements (`<dialog>`, `<details>`, `<input type="...">`, `<datalist>`, `<meter>`, `<progress>`) degrade gracefully on older browsers to plain containers.
- Provide a `<noscript>` block for pages that genuinely require JavaScript.

### CSS `@supports` and `CSS.supports()`

`@supports` is the cleanest way to guard a feature in CSS:

```css path=null start=null
.grid { display: block; }

@supports (display: grid) {
  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(20rem, 100%), 1fr));
    gap: 1rem;
  }
}

@supports (container-type: inline-size) {
  .card-wrap { container: card / inline-size; }
  @container card (inline-size < 24rem) { .card { grid-template-columns: 1fr; } }
}

@supports not (container-type: inline-size) {
  /* media-query fallback for the rare unsupporting browser */
}
```

In JavaScript, `CSS.supports('display: grid')` and `CSS.supports('selector(:has(*))')` let the same branches happen at runtime.

### JavaScript feature detection

Check for the feature, not the browser. User-agent sniffing is brittle and legally fraught.

```js path=null start=null
if ('startViewTransition' in document) {
  document.startViewTransition(() => updateDom());
} else {
  updateDom();
}

if (typeof Temporal !== 'undefined') {
  // use native Temporal
} else {
  // load polyfill
  await import('temporal-polyfill/global');
}

if (HTMLElement.prototype.showPopover) {
  // native popover
} else {
  // load polyfill or fall back to a click-outside handler
}
```

For dynamic imports of polyfills, gate the import behind the detection so supporting browsers never download them.

### Polyfills and ponyfills

- Polyfill — installs the feature globally (`Array.prototype.at`, `Temporal` on `globalThis`).
- Ponyfill — exports the feature as a module that callers import explicitly.

Prefer ponyfills when possible; polyfills can collide with future native implementations and inflate the bundle for users that do not need them. Load polyfills only after feature detection fails, and only for the features actually used.

Common maintained polyfills:

- `temporal-polyfill` or `@js-temporal/polyfill` — Temporal.
- `signal-polyfill` — TC39 Signals.
- `@oddbird/popover-polyfill` — popover attribute.
- `@oddbird/css-anchor-positioning` — anchor positioning.
- `container-query-polyfill` (Google Chrome Labs) — container size queries for pre-Safari 16.
- `urlpattern-polyfill` — `URLPattern`.
- `core-js` — catch-all ES polyfills; prefer targeted polyfills where possible.

## Feature-by-feature status (April 2026)

A quick reference for the non-obvious features used elsewhere in the guide. Verify against Baseline or Can I Use before relying on any of these; the Baseline feed updates monthly and the list below will age.

### Widely available (ship without guards)

- CSS Grid, Flexbox, `gap`, `aspect-ratio`, `clamp()`, `min()`, `max()`, container size queries, `:has()`, native CSS nesting, `@layer`, `:focus-visible`, subgrid, `text-wrap: balance`, `color-mix()`, `light-dark()`, logical properties.
- `image-set()`, `hyphens`, `hyphenate-character`, `@counter-style`, `contain-intrinsic-size`, overflow media queries (`overflow-block`, `overflow-inline`), `update` media query, `dirname` attribute on form inputs, `<link rel="modulepreload">`.
- Variable fonts, WOFF2, `font-display`, `fetchpriority`, `loading="lazy"`, `decoding="async"`, `<picture>` with AVIF/WebP, `inert`, `content-visibility: auto`, native lazy loading.
- `fetch`, `AbortController`, `IntersectionObserver`, `ResizeObserver`, `PerformanceObserver`, `structuredClone`, `URL`, `URLSearchParams`, `BroadcastChannel`, `navigator.storage`, Device Orientation events, `Array.prototype.at`, optional chaining, nullish coalescing.
- `<dialog>`, `<details>`, `<summary>`, `<datalist>`.

### Newly available (guard or accept a minor degradation)

Reached Baseline Newly Available between October 2025 and March 2026:

- `@scope` at-rule — Chrome 143+, Firefox 146+, Safari 26.2+.
- Invoker commands (`command` / `commandfor` attributes on `<button>`) — Chrome 135+, Firefox 144+, Safari 26.2+.
- `view-transition-class` — Chrome 125+, Firefox 144+.
- `scrollbar-color` — Chrome 121+, Firefox 64+, Safari 26.2+.
- `scrollend` event.
- `text-decoration-line: spelling-error` and `grammar-error`.
- `text-indent: each-line` and `text-indent: hanging`.
- `rcap`, `rch`, `rex`, `ric` root-relative CSS length units (Jan 2026).
- Navigation API (`navigation` global) — Chrome 102+, Firefox 147+, Safari 26.2+.
- JavaScript modules in service workers (Jan 2026).
- `document.caretPositionFromPoint()`.
- Active view transition pseudo-class (`:active-view-transition`).
- `@starting-style` and `transition-behavior: allow-discrete`.
- CSS `shape()` function — Chrome 135+, Firefox 148+, Safari 18.4+.
- Trusted Types API — Chrome 83+, Firefox 148+, Safari 26+.
- Zstandard (`Content-Encoding: zstd`) — Chrome 123+, Firefox 126+, Safari 26.3+.
- `Map.prototype.getOrInsert()` and `getOrInsertComputed()`.
- `Iterator.prototype.concat()`.
- WebTransport API — Chrome 97+, Firefox 114+, Safari 26.4+.
- Reporting API (`Reporting-Endpoints` header, `ReportingObserver`) — Chrome 96+, Firefox 149+, Safari 16.4+.
- Readable byte streams (`ReadableStream` with `{ type: "bytes" }`).
- `Atomics.waitAsync()`.
- `font-family: math` keyword.
- Event Timing and Largest Contentful Paint performance entries — both now Baseline Newly Available after Safari shipped them in 26.2.
- Temporal — Firefox 139+, Chrome 144+, Edge 144+. Safari not shipped as of April 2026; use polyfill where cross-browser support is required.

Still in Limited Availability (guard, polyfill, or accept simplified behavior):

- Anchor positioning — Chromium complete, Firefox 147+, Safari partial.
- `field-sizing: content` — Chromium and Safari, Firefox not shipped.
- Scroll-driven animations (`animation-timeline: view()`, `scroll()`) — Chromium only.
- `interpolate-size: allow-keywords` / `calc-size()` — Chromium only.
- Container style queries (`@container style(...)`) — Chromium and Safari, no Firefox.
- CSS `@function` and `if()` — Chromium only, experimental.
- Signals (TC39) — Stage 1 proposal; no engine ships natively. Polyfill available.
- Declarative Shadow DOM (`<template shadowrootmode="open">`) — all four engines support it in reasonably current versions; check browser versions if legacy support matters.
- `CustomElementRegistry` scoped registries.
- `@property` with complex types (basics are interoperable; complex syntax edge cases vary).
- `URLPattern` — Chromium and Safari, Firefox shipping soon.
- `WebGPU` — Chromium shipped, Safari in progress, Firefox partial.

For every feature in the newly-available and limited-availability tiers, pick one strategy: feature-detect and fall back, polyfill, or accept a simplified experience for unsupporting browsers. Do not ship a broken page.

## Practical fallback patterns

### View Transitions

```js path=null start=null
const swap = () => updateDom();
if (document.startViewTransition) {
  document.startViewTransition(swap);
} else {
  swap();
}
```

No CSS fallback is needed; the animation is additive polish.

### Container queries

Wrap container-driven layout in `@supports`; provide a media-query fallback for the one case that matters most on the page. Most modern audiences already pass the Baseline threshold, so this is increasingly unnecessary.

### Anchor positioning

Feature-detect and either polyfill (OddBird's polyfill) or hand-place the popover with `position: absolute` plus JavaScript `getBoundingClientRect`.

### Animating to `auto`

`interpolate-size: allow-keywords` is Chromium-only at time of writing. Guard with `@supports` and use a JavaScript height-measurement fallback or a `max-height` transition (accepting the usual max-height caveats) for non-supporting browsers.

### Popover

If popover is not supported, load `@oddbird/popover-polyfill` or replace the element with a CSS `:hover`/`:focus-within` disclosure and a click-outside handler.

### Modern image formats

Serve AVIF, WebP, and JPEG together via `<picture>`:

```html path=null start=null
<picture>
  <source type="image/avif" srcset="/hero.avif" />
  <source type="image/webp" srcset="/hero.webp" />
  <img src="/hero.jpg" alt="…" width="1200" height="630" />
</picture>
```

Every modern browser picks the first supported format; older browsers fall back to `<img>`.

### Fonts

Preload the WOFF2 primary. Use `font-display: swap` with a metric-matched system fallback or `font-display: optional` for decorative faces. No separate handling is needed for browsers that do not support variable fonts; the static fallback file handles them.

## Testing across browsers

Automated checks do not replace human verification on real browsers. A sustainable test plan:

- Local development — the latest stable of the primary browser, plus Safari on macOS for any WebKit-specific rendering. Safari Technology Preview for early detection of regressions.
- Device testing — a real iPhone and a real Android mid-tier phone, or at minimum Chrome DevTools device emulation plus Safari Responsive Design Mode.
- Cross-browser matrices — BrowserStack, Sauce Labs, LambdaTest, or Playwright with multiple browsers wired into CI for the agreed support matrix.
- Regression testing — visual diffs per browser via Percy, Chromatic, or Playwright's `toHaveScreenshot`, limited to the support matrix.
- Accessibility testing — at least one screen reader per major OS (VoiceOver on macOS/iOS, NVDA or JAWS on Windows, TalkBack on Android).
- Field measurement — Real-User Monitoring (`web-vitals`) segmented by browser and version to catch regressions invisible in the lab.

Budget concrete time per release for the matrix. "It works on Chrome" is a starting point, not a QA plan.

## CI enforcement

- Share a `browserslist` configuration across the whole toolchain so Autoprefixer, Babel (`@babel/preset-env`), PostCSS Preset Env, ESLint, and Stylelint all target the same browsers. Configure it in `package.json` or `.browserslistrc`.
- Run Playwright or WebDriver tests against the agreed browser set on every pull request.
- Fail the build if a new CSS or JS feature is used without a `@supports` guard or feature detection when the target matrix does not support it. Current tooling (April 2026):
  - `eslint-plugin-compat` (v7.x, `npm i -D eslint-plugin-compat`) lints Web APIs and ES APIs against `browserslist`.
  - `stylelint-no-unsupported-browser-features` (v8.x, maintained by RJWadley; `npm i -D stylelint-no-unsupported-browser-features`) uses `doiuse` + `caniuse`.
  - `stylelint-browser-compat` (beta) and `stylelint-plugin-use-baseline` are newer alternatives built on `@mdn/browser-compat-data` and `web-features` respectively.
  - ESLint's built-in Baseline CSS integration enforces Baseline membership directly.
- Keep `caniuse-lite` fresh: schedule a recurring `npx update-browserslist-db@latest` or wire up `browserslist-update-action` on GitHub.
- Track bundle size for polyfills and keep them off the critical path; load polyfills only after detection fails and prefer dynamic `import()`.
- Segment Real-User Monitoring (`web-vitals`) by browser and version to catch regressions that only appear on one engine.

## Resources

- `web.dev/baseline` — current Baseline threshold and feature classification.
- `web.dev/blog` — monthly Baseline digest of newly-available and widely-available features.
- `web-platform-dx.github.io/web-features-explorer` — per-feature Baseline browser data.
- `webstatus.dev` — cross-browser feature status dashboard with Baseline queries.
- `caniuse.com` — per-feature version history across browsers.
- `wpt.fyi` and `wpt.fyi/interop-2026` — Web Platform Tests results and the current Interop effort.
- `developer.mozilla.org` — MDN browser-compatibility tables with Baseline badges.
- `browsersl.ist` and `browserslist.dev` — interactive query builders for `browserslist` configuration.
- `gs.statcounter.com` — global and regional browser market-share statistics.
- `chrome.dev/google-analytics-baseline-checker` — converts a GA4 export into Baseline coverage numbers.
- `github.com/web-platform-dx/baseline-browser-mapping` — npm module and CSV for joining analytics data to Baseline tiers.
- `rumarchive.com/insights/#baseline` — RUM Archive insights filtered by Baseline target and country.

## The shortest rule

Pick a support matrix, write it down, feature-detect everything outside "widely available," test on real browsers in that matrix, and never assume "works in Chrome" means "works." Everything else in this chapter is a specialization of those five steps.
