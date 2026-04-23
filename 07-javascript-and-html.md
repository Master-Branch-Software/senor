# JavaScript and HTML

The platform keeps gaining capabilities that replace libraries. Using what the browser and language already provide shrinks bundles, reduces complexity, and ages better than any framework choice.

## Modern HTML worth using

The HTML specification has quietly added powerful elements. Prefer them over custom JavaScript implementations.

- `<dialog>` — modal and non-modal dialogs with `showModal()`, focus trap, `::backdrop`, Escape-to-close.
- `<details>` / `<summary>` — zero-JavaScript disclosure. Combine with `name="group"` for exclusive accordions.
- `<popover>` attribute — declarative tooltips, menus, and info cards with built-in show/hide and light dismiss.
- `invoker` commands and `command`/`commandfor` attributes — declarative wiring of buttons to actions (open, close, toggle) on target elements.
- `<input type="search">`, `<input type="tel">`, `<input type="email">`, `<input type="url">`, `<input type="date">`, `<input type="time">`, `<input type="color">` — correct mobile keyboards and native pickers.
- `<datalist>` — suggest-as-you-type combobox without a library.
- Native lazy loading via `loading="lazy"` on `<img>` and `<iframe>`.
- `fetchpriority="high"`, `fetchpriority="low"` for hinting resource priority.
- `inert` attribute — remove subtrees from focus, accessibility tree, and click handling.
- `aria-live` regions for announcements.
- `content-visibility: auto` — skip layout and paint for off-screen content.
- `<picture>` — art direction plus format negotiation.
- `<source srcset="…" type="image/avif">` inside `<picture>` for AVIF/WebP fallback chains.

Use these first; reach for JavaScript only when the native primitive cannot express the intent.

## ECMAScript 2025 and 2026

ECMAScript ships yearly. The 2025 additions (published June 2025) and Stage-4 2026 candidates worth learning:

### Iterator helpers (ES2025)

Map, filter, take, drop, and more directly on any iterator. Lazy evaluation; no intermediate arrays.

```js path=null start=null
const result = Iterator.from(array)
  .map(x => x * 2)
  .filter(x => x > 10)
  .take(3)
  .toArray();
```

### Set methods (ES2025)

`intersection`, `union`, `difference`, `symmetricDifference`, `isSubsetOf`, `isSupersetOf`, `isDisjointFrom` on `Set`. Eliminates a common reason to reach for Lodash or immutable.js for set operations.

### `RegExp.escape()` (ES2025)

After fifteen years, a standard way to escape user-supplied strings before feeding them to a regex.

```js path=null start=null
const re = new RegExp(RegExp.escape(userInput), "g");
```

### Regex flag modifiers (ES2025)

Inline `(?i:...)` and `(?-i:...)` to enable or disable flags for parts of a regex.

### `Promise.try()` (ES2025)

Treat sync throws and async rejections uniformly.

```js path=null start=null
Promise.try(() => maybeSyncOrAsync())
  .then(handle)
  .catch(report);
```

### Import attributes

A supported syntax for typed imports:

```js path=null start=null
import data  from "./data.json" with { type: "json" };
import sheet from "./styles.css" with { type: "css" };
```

CSS module scripts (supported now in Chromium and Firefox 147+) allow importing CSS as a constructable stylesheet — useful for applying scoped styles to a web component's shadow root without an extra fetch.

### Temporal API (landing in ES2026)

A modern replacement for `Date`. Covers plain dates, plain times, time zones, instants, durations, and calendars; immutable, well-typed, and correct.

```js path=null start=null
const now   = Temporal.Now.zonedDateTimeISO();
const later = now.add({ hours: 2, minutes: 30 });
const diff  = later.since(now, { largestUnit: "minute" });
```

Supported natively in most engines as of 2026; use a polyfill where needed. Any new code should prefer Temporal over `Date`.

## Signals (TC39 Stage 1, 2026)

The Signals proposal introduces a standard reactive primitive to JavaScript: `Signal.State` and `Signal.Computed`. Framework authors (Angular, Vue, Preact, Solid, Qwik, Svelte, Ember, MobX, Lit) collaborated on the proposal so frameworks can build on a common base.

```js path=null start=null
import { Signal } from "signal-polyfill";

const count    = new Signal.State(0);
const doubled  = new Signal.Computed(() => count.get() * 2);

console.log(doubled.get()); // 0
count.set(5);
console.log(doubled.get()); // 10
```

Signals are automatically tracked; dependencies do not need to be declared. Effects are intentionally not part of the proposal (they are framework concerns). Until native, the `signal-polyfill` package tracks the spec. Frameworks that ship signals today (Angular, Vue, Solid, Preact) can continue to be used; code that is framework-agnostic can now share state across frameworks through the polyfill.

## Web components

Native custom elements with optional Shadow DOM. Useful when a component must work across frameworks, inside a server-rendered page, or standalone.

```js path=null start=null
class AlertMachine extends HTMLElement {
  connectedCallback() {
    this.shadowRoot ?? this.attachShadow({ mode: "open" });
    this.shadowRoot.innerHTML = `
      <style>button { font: inherit; }</style>
      <button part="trigger"><slot></slot></button>
    `;
    this.shadowRoot.querySelector("button").addEventListener("click", () => {
      this.dispatchEvent(new CustomEvent("alert", { bubbles: true }));
    });
  }
}
customElements.define("alert-machine", AlertMachine);
```

### Shadow DOM vs Light DOM

- Shadow DOM encapsulates markup and styles. Outside CSS does not leak in; component CSS does not leak out. Use when a component must look and behave the same regardless of where it is mounted (design system, embeddable widget).
- Light DOM ("HTML with superpowers") means a custom element whose children live in the regular DOM. No encapsulation, but natural to style and simpler. Use when the main value is attaching behavior to existing markup.

### Styling Shadow DOM from the outside

- Expose CSS custom properties (`--ag-space-2`) for broad theming.
- Expose named `part`s (`::part(ag-input)`) for targeted overrides.
- Use `:host`, `:host()`, `:host-context()` for scheme or context-dependent styles.

### Form-associated custom elements

Custom elements opt into form participation with `static formAssociated = true` plus `attachInternals()`. They must then implement `setFormValue`, `formResetCallback`, `formDisabledCallback`, and validation through `ElementInternals.setValidity()`. Delegate to an inner native input where possible to inherit the browser's constraint validation engine.

### Declarative Shadow DOM

A `<template shadowrootmode="open">` inside a custom element tag allows server-rendered shadow DOM — no JavaScript required for the initial paint.

```html path=null start=null
<stop-watch>
  <template shadowrootmode="open">
    <span>00:00:00</span>
    <button>Start</button>
  </template>
</stop-watch>
```

### When not to use web components

- When the primary consumers are inside one JavaScript framework and no cross-framework sharing is planned. The framework's component model is more ergonomic.
- When needed features (form participation, keyboard interactions, announce on change) require significant boilerplate you would not have to write otherwise.

### Lit

A ~6 KB library that makes writing custom elements pleasant. Provides templating (`html`…``), reactive properties, decorators, and a standard dev experience.

```js path=null start=null
import { LitElement, html, css } from "lit";
import { customElement, property } from "lit/decorators.js";

@customElement("my-counter")
export class MyCounter extends LitElement {
  static styles = css`button { font: inherit; }`;
  @property({ type: Number }) count = 0;

  render() {
    return html`
      <button @click=${() => this.count++}>
        Count: ${this.count}
      </button>
    `;
  }
}
```

Recommended for building web components at any non-trivial scale.

## Runtime selection

Three viable server-side JavaScript runtimes. Pick based on constraints.

### Node.js

The default. Largest ecosystem, deepest tooling, broad hiring pool, longest LTS track record. Current Node (≥ 22) includes:

- Native TypeScript execution via type stripping.
- Built-in test runner (`node:test`), `fetch`, `WebStreams`.
- Experimental permission model.
- Native `.env` loading (`--env-file`).

Pick Node.js when compatibility, stability, or ecosystem breadth dominates. It is the correct default for most business applications.

### Bun

JavaScriptCore-based runtime, bundler, test runner, and package manager in one binary. Highlights:

- Fastest cold start and install times of the three.
- Near-complete Node compatibility (~98% of npm packages).
- Native TypeScript and JSX execution with zero configuration.
- Native APIs (`Bun.serve`, `Bun.file`, `Bun:sqlite`, `Bun.password`) outperform Node equivalents.

Pick Bun for performance-sensitive microservices, CLI tools, CI pipelines where install time matters, or new projects where its tooling simplicity saves configuration time.

### Deno

Rust-based runtime with first-class TypeScript, web-standards APIs, and a granular permission model.

- `deno run` requires explicit `--allow-net`, `--allow-read`, etc.; a default Deno script can do nothing.
- `deno compile` produces a self-contained binary — ideal for CLI distribution.
- Built-in formatter, linter, test runner, doc generator.
- Deno KV for zero-config global key-value storage.
- Deno Deploy for edge deployment.

Pick Deno when security (multi-tenant code execution, auditable scripts), zero-config TypeScript, or single-binary distribution are primary constraints.

### Cross-runtime compatibility (WinterCG)

`fetch`, `Request`, `Response`, `ReadableStream`, `URL`, `crypto.subtle`, and related Web-standard APIs are now supported identically across Node, Bun, Deno, Cloudflare Workers, and other edge runtimes. Writing handlers against those APIs means the same code runs across environments without change.

## HTTP request patterns

- Use `fetch` with `AbortController` for cancellation.
- Set reasonable timeouts. No fetch should run indefinitely.
- Handle non-2xx responses explicitly; `fetch` does not reject on 4xx/5xx.
- Prefer streaming responses for large payloads.
- Use `Cache-Control` headers aggressively on GETs.
- Never include user secrets in URLs; use `Authorization` headers.

## Observable and Intersection APIs

Native APIs that replace many third-party libraries:

- `IntersectionObserver` — scroll-triggered behavior, lazy loading, infinite scroll.
- `ResizeObserver` — reactive to element size changes.
- `MutationObserver` — react to DOM mutations (rarely needed; signals and framework state are usually better).
- `PerformanceObserver` — collect Core Web Vitals and other metrics.
- `AbortController` / `AbortSignal` — cancel any asynchronous work.
- `requestIdleCallback`, `requestAnimationFrame`, `scheduler.yield()` — schedule low-priority or frame-aligned work.

## Progressive enhancement tactics

The browser does most of the work when given the chance:

- HTML forms with `action` and `method` always work without JavaScript. Layer client validation on top.
- Links with `href` always navigate. Intercept them only when single-page routing truly benefits.
- `<details>` and `<dialog>` replace handwritten expand/collapse and modal logic.
- `<input type="search">` with `<datalist>` replaces small typeahead libraries.
- CSS `:has()`, `:user-invalid`, container queries remove the need for many JavaScript state toggles.

## Security essentials

- Serve over HTTPS, always.
- Apply a strict Content Security Policy. Start with `default-src 'self'` and allow explicitly.
- Set security headers: `Strict-Transport-Security`, `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy` to restrict features.
- Avoid `eval`, `Function` constructor, and `innerHTML` with untrusted input. Use `textContent`, `setAttribute`, or template libraries with auto-escaping.
- Validate and sanitize all user input on the server; never trust client-side validation alone.
- Use `SameSite=Lax` or `Strict` cookies; use `HttpOnly` for session cookies.
- Rotate secrets on a schedule; never commit them.

## TypeScript

- Use TypeScript on any project expected to live longer than a weekend.
- Keep `strict` mode on. `noImplicitAny`, `strictNullChecks`, and `noUncheckedIndexedAccess` catch entire bug classes.
- Prefer inference. Avoid redundant type annotations.
- Model domain data with discriminated unions; avoid `any` and use `unknown` with narrowing when genuinely uncertain.
- Derive types from single sources of truth (Zod, Valibot, or database schemas) rather than duplicating them.
- Treat the type system as a design tool, not a syntax tax.

## Date and time discipline

Until Temporal is universally available:

- Store all timestamps in UTC (ISO 8601) on the server and in the database.
- Convert to the user's time zone at render time, not at storage time.
- Never store local time without a time-zone identifier.
- Use `Intl.DateTimeFormat` for locale-aware formatting; avoid hand-rolling formats.
- For durations and calendar math, adopt `Temporal` now behind a polyfill; it will age well.

## Keep the platform in view

When a new library promises to solve a problem, ask first whether the browser already can. Modern CSS, HTML, and JavaScript have eliminated entire library categories — state management for simple cases, animation for most interactions, date math, validation for forms, clipboard access, geolocation, lazy loading, dialogs, popovers, drag handles. Leaning on the platform produces code that loads faster, runs faster, and survives rewrites of its dependencies.
