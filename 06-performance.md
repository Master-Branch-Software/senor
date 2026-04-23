# Performance

Performance is a feature. Users abandon slow pages; Google ranks slow pages lower; low-end devices simply cannot run bloated JavaScript. Treat a 50 KB regression as seriously as a visual bug.

## Core Web Vitals

Google's Core Web Vitals are the canonical field metrics. Thresholds are measured at the 75th percentile of real user visits.

- LCP — Largest Contentful Paint. Time until the largest visible element finishes rendering. Good ≤ 2.5 s. Poor > 4.0 s.
- INP — Interaction to Next Paint. The worst interaction latency during the session. Good ≤ 200 ms. Poor > 500 ms. Replaced FID in March 2024.
- CLS — Cumulative Layout Shift. Sum of unexpected layout shift scores. Good ≤ 0.1. Poor > 0.25.

Lab metrics (Lighthouse) are helpful for debugging but are not what Google uses for ranking. Field data from the Chrome User Experience Report (CrUX) is what matters.

## LCP: getting the biggest element on screen fast

The LCP element is almost always the hero image, a large heading, or a video poster. The LCP timeline has four phases: TTFB, resource load delay, resource load duration, element render delay.

Practical fixes, in order of impact:

1. Serve the LCP image from a CDN with a fast TTFB.
2. Preload it in the document head:

```html path=null start=null
<link
  rel="preload"
  as="image"
  href="/hero.webp"
  fetchpriority="high"
  imagesrcset="/hero-640.webp 640w, /hero-1280.webp 1280w, /hero-1920.webp 1920w"
  imagesizes="(max-width: 640px) 100vw, 1280px"
/>
```

3. Use `fetchpriority="high"` and `loading="eager"` on the `<img>`:

```html path=null start=null
<img
  src="/hero-1280.webp"
  srcset="/hero-640.webp 640w, /hero-1280.webp 1280w, /hero-1920.webp 1920w"
  sizes="(max-width: 640px) 100vw, 1280px"
  width="1280"
  height="720"
  alt="Product preview showing the dashboard"
  loading="eager"
  fetchpriority="high"
/>
```

4. Convert hero images to AVIF or WebP. WebP is the pragmatic default; AVIF adds another 20–25% reduction at the cost of slower decoding. Wrap AVIF in `<picture>` with a WebP source and JPEG fallback when file size is critical.
5. Move render-blocking CSS out of the critical path. Inline critical CSS; defer the rest with `media="print"` swap or `preload` with `onload`.
6. Defer non-critical JavaScript with `defer` or `async`. Never place synchronous `<script>` tags in `<head>` without good reason.
7. For text-based LCP, preload the primary font file and use `font-display: swap` with matched-metric fallbacks, or `font-display: optional`.
8. Server-render above-the-fold content so text is present before hydration.

Never:

- Apply `loading="lazy"` to the LCP image.
- Hide the LCP element behind a client-side route guard or authentication check that runs before first paint.
- Place the LCP image in a CSS `background-image`. The browser cannot discover it until the CSS has loaded; use an inline `<img>`.

## INP: staying responsive under interaction

INP measures the worst input-to-paint latency across the entire session. It has three phases: input delay, event processing, presentation delay.

The core rules:

- Keep tasks short. Any JavaScript task over 50 ms is a "long task." Break long work with `scheduler.yield()` (preferred), `queueMicrotask`, `requestIdleCallback`, or by deferring to a Web Worker.
- Debounce expensive handlers. Search inputs, resize handlers, and scroll handlers should not run on every raw event.
- Avoid forced synchronous layout. Reading `offsetHeight` or `getBoundingClientRect` after writing a DOM property forces the browser to recalculate layout immediately; batch reads, then writes.
- Use `requestAnimationFrame` for visual updates tied to the next frame.
- Move heavy computation off the main thread. Use Web Workers for parsing, sorting, filtering, or any CPU-bound work.

React / framework patterns:

- Use `startTransition` or `useDeferredValue` to mark non-urgent updates.
- Virtualize long lists (`@tanstack/virtual`, `react-window`).
- Avoid unnecessary re-renders: memoize expensive children, split state so unrelated components do not re-render together.
- Hydrate selectively. If the framework supports partial hydration or islands (Astro, Qwik, React Server Components), use it.

Third-party scripts are the single most common cause of poor INP. Every analytics script, chat widget, ad tag, and A/B-test SDK competes for the main thread. Each one should pass a cost-benefit test; defer or remove those that fail it.

Techniques for third parties:

- Load after user interaction or idle (`strategy="lazyOnload"` in Next.js Script).
- Use the facade pattern: render a static placeholder that replaces itself with the real embed only when the user interacts. Applies to video players, maps, and chat widgets.
- Never synchronous-load third-party JavaScript in `<head>`.

## CLS: no unexpected movement

Cumulative Layout Shift is almost always caused by content without reserved space. The fixes are well-defined.

- Every `<img>`, `<video>`, and `<iframe>` has explicit `width` and `height` attributes. The browser uses them to compute an intrinsic aspect ratio and reserve space before the resource loads.
- Web fonts should not shift layout when they load. Use `font-display: swap` with metric-adjusted fallbacks (`size-adjust`, `ascent-override`, `descent-override`, `line-gap-override`) or `font-display: optional`. Tools like Fontaine generate these overrides automatically.
- Reserve space for ad slots and embeds with `min-height` or `aspect-ratio`.
- Do not inject content above existing content after initial render (cookie banners, notification bars). Overlay them, or render them from the first paint.
- Avoid animating layout-triggering properties (`width`, `height`, `top`, `left`, `margin`). Use `transform` and `opacity` instead.
- Client-rendered content should render skeletons with matching dimensions; otherwise a late mount triggers a visible shift.

## Image optimization

Images are the single largest bytes on most pages. Optimize in layers.

1. Resize to display dimensions. A 600-px rendered image does not need a 2400-px source. Generate multiple widths at build time.
2. Use `srcset` and `sizes` for responsive delivery.

```html path=null start=null
<img
  src="/photo-800.webp"
  srcset="/photo-400.webp 400w, /photo-800.webp 800w, /photo-1200.webp 1200w"
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 1200px"
  width="1200"
  height="800"
  alt="Succinct description"
  loading="lazy"
  decoding="async"
/>
```

3. Pick a format intentionally.
   - WebP — default for photography (~30% smaller than JPEG; universal support).
   - AVIF — hero images where every kilobyte matters; ~50% smaller than JPEG but 2–3× slower to decode; serve with `<picture>` fallback.
   - SVG — logos, icons, illustrations; scale-free.
   - PNG — transparency where WebP is not acceptable (rare).
4. Lazy-load below-the-fold images with `loading="lazy"`. Use `decoding="async"` to move decode off the main thread. Never lazy-load the LCP image.
5. Prefer an image CDN (Cloudinary, Imgix, Cloudflare Images, Vercel Image, Netlify Image) for on-the-fly resizing and format negotiation. Framework-native components (`next/image`, `Astro <Image>`, `@nuxt/image`) integrate with these CDNs.
6. Use `content-visibility: auto` on off-screen image-heavy sections to skip layout and paint until near the viewport.

## Font loading

Fonts are a common cause of both CLS and slow first paint.

- Self-host fonts when possible. A CDN adds DNS, TCP, and TLS round-trips; self-hosting avoids them.
- Preload the primary font used above the fold:

```html path=null start=null
<link
  rel="preload"
  href="/fonts/Inter.woff2"
  as="font"
  type="font/woff2"
  crossorigin
/>
```

- Use WOFF2 only. It is universally supported and significantly smaller than TTF or WOFF.
- Subset to the character ranges actually used (Latin, Latin Extended, etc.). Tools: `glyphhanger`, `subfont`, `fonttools`.
- Choose the right `font-display`:
  - `swap` — fastest visible text; possible FOUT.
  - `fallback` — brief invisible period (~100 ms), then swap if arrives, else stay on fallback.
  - `optional` — brief invisible period, then use the font only if already cached; zero CLS in this mode.
- Generate metric-adjusted fallbacks with `size-adjust`, `ascent-override`, `descent-override`, `line-gap-override` to keep layout stable across loading states.
- Use `next/font` (Next.js) or equivalent to automate self-hosting and metric matching.

## Third-party scripts: the silent killer

A single unmanaged third-party script can wipe out every other optimization. Policies that work:

- Every third party is evaluated for business value, privacy impact, and performance cost.
- Scripts load with `async` or `defer` by default; never synchronous in `<head>`.
- Analytics loads after first paint or on interaction.
- Chat widgets load on user intent (scroll past, click of a placeholder, idle callback).
- Ad slots have reserved space so loading does not shift layout.
- Consider server-side analytics where possible (no client-side script at all).

## Critical rendering path

The browser cannot paint until it has parsed enough HTML and CSS. Shrink the critical path:

- Inline CSS necessary for the first viewport. Load the rest asynchronously.
- Keep initial HTML small (< 50 KB gzipped is a good goal).
- Use `defer` on scripts so they execute after HTML parsing completes.
- Use `preconnect` and `dns-prefetch` for origins that will serve critical resources:

```html path=null start=null
<link rel="preconnect" href="https://cdn.example.com" crossorigin />
<link rel="dns-prefetch" href="https://cdn.example.com" />
```

- Use `modulepreload` for ES modules that will be imported soon.
- Use `103 Early Hints` on the server where supported to start client-side work before the full response completes.
- Enable HTTP/2 or HTTP/3 and Brotli compression.

## Caching

- Fingerprint static assets (`/static/main.abc123.js`) and cache them for one year (`Cache-Control: public, max-age=31536000, immutable`).
- HTML: short or zero max-age, with `must-revalidate` or `stale-while-revalidate`. Revalidate on every request.
- API responses: pick `s-maxage` plus `stale-while-revalidate` to let CDNs serve stale while refreshing.
- Use a service worker only when the app genuinely benefits (offline support, custom caching strategy). Service workers add complexity and can regress performance if misconfigured.

## JavaScript bundle discipline

- Audit the bundle monthly. Tools: `source-map-explorer`, `rollup-plugin-visualizer`, `@next/bundle-analyzer`, `webpack-bundle-analyzer`.
- Replace heavy libraries with lighter alternatives or native APIs. `date-fns` or `Temporal` beats `moment`. Native `fetch` beats `axios`. Native `Intl` beats third-party locale libraries for most needs.
- Tree-shake by preferring ES modules and deep imports (`import debounce from 'lodash-es/debounce'` rather than the whole library).
- Route-split by default; component-split large interactive widgets.
- Remove polyfills for features available in the current browser baseline.

## Rendering strategy

Pick the strategy that best matches the content, then stay consistent:

- SSG (static) — content that rarely changes (marketing pages, blog posts, docs). Cheapest TTFB.
- ISR / on-demand revalidation — mostly-static content that changes occasionally (product pages, listings).
- SSR — content that varies per request or user (dashboards, authenticated pages).
- Edge rendering — global, dynamic, latency-sensitive pages (authentication, routing, personalization). Cloudflare Workers, Vercel Edge, Deno Deploy.
- Client-only — private apps behind a login, where SEO and first load are not the priority.

Most marketing sites should be SSG by default, with hybrid SSR for dynamic regions.

## Monitoring

Field monitoring is required for all production sites. The minimum stack:

- Real-User Monitoring (RUM) of Core Web Vitals with the `web-vitals` library, sending beacons to an analytics endpoint.
- CrUX-based dashboards (PageSpeed Insights, Chrome UX Report on BigQuery).
- Lighthouse CI (or equivalent) in CI/CD, enforcing performance budgets on pull requests.
- Synthetic monitoring for critical pages from multiple geographies.

Example RUM integration:

```js path=null start=null
import { onLCP, onINP, onCLS } from "web-vitals/attribution";

const report = (metric) => {
  navigator.sendBeacon(
    "/vitals",
    JSON.stringify({
      name: metric.name,
      value: metric.value,
      id: metric.id,
      url: location.pathname,
      attribution: metric.attribution,
    })
  );
};

onLCP(report);
onINP(report);
onCLS(report);
```

Set performance budgets:

- HTML: ≤ 50 KB gzipped.
- CSS: ≤ 50 KB gzipped.
- JS initial: ≤ 150 KB gzipped for marketing pages; ≤ 200 KB for apps.
- Images: ≤ 1 MB per page total, ≤ 200 KB per image above the fold.
- Third-party scripts: count and weight tracked, with a hard ceiling.

Fail the build when budgets are exceeded without explicit override.

## Progressive enhancement

A page should work when JavaScript fails to load, in a reduced form. That means:

- Links and forms work server-side.
- Content is visible without JavaScript.
- Client-side enhancements are layered on top of HTML that already communicates the intent.

Progressive enhancement is not a nostalgic preference; it is the fastest and most resilient way to ship. A page that renders instantly without JavaScript is a page that renders instantly with JavaScript, once the JavaScript arrives.
