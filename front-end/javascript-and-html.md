# JavaScript and HTML

The platform keeps gaining capabilities that replace libraries. Using what the browser and language already provide shrinks bundles, reduces complexity, and ages better than any framework choice.

## Modern HTML worth using

The HTML specification has quietly added powerful elements. Prefer them over custom JavaScript implementations.

- `<dialog>` — modal and non-modal dialogs with `showModal()`, focus trap, `::backdrop`, Escape-to-close.
- `<details>` / `<summary>` — zero-JavaScript disclosure. Combine with `name="group"` for exclusive accordions.
- `popover` attribute — declarative tooltips, menus, and info cards with built-in show/hide and light dismiss.
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

ECMAScript ships yearly. ES2025 was approved by the Ecma General Assembly on June 25, 2025. ES2026 candidates are already at Stage 4 and scheduled for the June 2026 release.

### Iterator helpers (ES2025)

Map, filter, take, drop, and more directly on any iterator. Lazy evaluation; no intermediate arrays.

```js path=null start=null
const result = Iterator.from(array)
  .map((x) => x * 2)
  .filter((x) => x > 10)
  .take(3)
  .toArray();
```

### Set methods (ES2025)

`intersection`, `union`, `difference`, `symmetricDifference`, `isSubsetOf`, `isSupersetOf`, `isDisjointFrom` on `Set`. Eliminates a common reason to reach for Lodash or immutable.js for set operations.

### `RegExp.escape()` (ES2025)

After fifteen years, a standard way to escape user-supplied strings before feeding them to a regex.

```js path=null start=null
const re = new RegExp(RegExp.escape(userInput), 'g');
```

### Regex flag modifiers and duplicate named groups (ES2025)

Inline `(?i:...)` and `(?-i:...)` enable or disable flags for parts of a regex. Duplicate named capture groups are also legal across alternatives, so `/(?<id>a+)|(?<id>b+)/` is valid in ES2025 where it was a syntax error before.

### `Promise.try()` (ES2025)

Treat sync throws and async rejections uniformly.

```js path=null start=null
Promise.try(() => maybeSyncOrAsync())
  .then(handle)
  .catch(report);
```

### Import attributes and JSON modules (ES2025)

A supported syntax for typed imports, plus native JSON module support:

```js path=null start=null
import data from './data.json' with { type: 'json' };
import sheet from './styles.css' with { type: 'css' };
```

CSS module scripts (supported in Chromium and Firefox 147+) allow importing CSS as a constructable stylesheet — useful for applying scoped styles to a web component's shadow root without an extra fetch.

### `Float16Array` (ES2025)

A half-precision typed array alongside `Float32Array` and `Float64Array`, with matching `DataView.prototype.getFloat16` / `setFloat16` and `Math.f16round`. Useful for memory-constrained numeric workloads: WebGL/WebGPU textures, ML inference weights, audio buffers.

### Temporal API (Stage 4; shipping in ES2026)

A modern replacement for `Date`. Covers plain dates, plain times, time zones, instants, durations, and calendars; immutable, well-typed, and correct.

```js path=null start=null
const now = Temporal.Now.zonedDateTimeISO();
const later = now.add({ hours: 2, minutes: 30 });
const diff = later.since(now, { largestUnit: 'minute' });
```

Status: reached Stage 4 and is being merged into ECMA-262 for ES2026. Shipped natively in Firefox 139 (May 2025) and Chrome 144 / Edge 144 (January 2026). Safari and iOS Safari do not support Temporal as of April 2026, so cross-browser code should still use `@js-temporal/polyfill` or `temporal-polyfill`. Any new code should prefer Temporal over `Date` through the polyfill.

## Signals (TC39 Stage 1, 2026)

The Signals proposal introduces a standard reactive primitive to JavaScript: `Signal.State` and `Signal.Computed`. Framework authors (Angular, Vue, Preact, Solid, Qwik, Svelte, Ember, MobX, Lit) collaborated on the proposal so frameworks can build on a common base.

```js path=null start=null
import { Signal } from 'signal-polyfill';

const count = new Signal.State(0);
const doubled = new Signal.Computed(() => count.get() * 2);

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
    this.shadowRoot ?? this.attachShadow({ mode: 'open' });
    this.shadowRoot.innerHTML = `
      <style>button { font: inherit; }</style>
      <button part="trigger"><slot></slot></button>
    `;
    this.shadowRoot.querySelector('button').addEventListener('click', () => {
      this.dispatchEvent(new CustomEvent('alert', { bubbles: true }));
    });
  }
}
customElements.define('alert-machine', AlertMachine);
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
import { LitElement, html, css } from 'lit';
import { customElement, property } from 'lit/decorators.js';

@customElement('my-counter')
export class MyCounter extends LitElement {
  static styles = css`
    button {
      font: inherit;
    }
  `;
  @property({ type: Number }) count = 0;

  render() {
    return html` <button @click=${() => this.count++}>Count: ${this.count}</button> `;
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

Use TypeScript on any project expected to live longer than a weekend. The cost of adding types after the fact exceeds the cost of writing them from day one.

### Recommended `tsconfig` baseline

```json path=null start=null
{
  "compilerOptions": {
    "target": "ES2023",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "lib": ["ES2023", "DOM", "DOM.Iterable"],
    "jsx": "preserve",

    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noImplicitOverride": true,
    "noPropertyAccessFromIndexSignature": true,
    "noFallthroughCasesInSwitch": true,
    "noImplicitReturns": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,

    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "isolatedModules": true,
    "skipLibCheck": true,
    "resolveJsonModule": true,
    "verbatimModuleSyntax": true
  }
}
```

Why each setting matters:

- `noUncheckedIndexedAccess` forces narrowing after `array[i]` and `record[key]`. Removes an entire class of undefined bugs.
- `exactOptionalPropertyTypes` distinguishes `prop?: string` (omit or string) from `prop: string | undefined` (must be present). Rarely enabled, high signal.
- `verbatimModuleSyntax` rejects implicit type-only imports; import values with `import` and types with `import type`. Required for bundlers that erase types at parse time.
- `isolatedModules` ensures every file can be transpiled alone, which matches how modern bundlers and Bun/Deno process TypeScript.
- `noPropertyAccessFromIndexSignature` distinguishes `obj.x` (known) from `obj['x']` (index-signature lookup). Keeps intent explicit.

Use project references (`references` + `composite: true`) for monorepos. Split types and runtime into separate `tsconfig.build.json` and `tsconfig.json` when the editor needs a wider view than production builds.

### Branded types

Brands prevent accidental mixing of structurally identical primitives (user IDs, order IDs, slugs, emails).

```ts path=null start=null
type Brand<T, B> = T & { readonly __brand: B };

type UserId = Brand<string, 'UserId'>;
type OrderId = Brand<string, 'OrderId'>;

const asUserId = (value: string): UserId => value as UserId;

function loadUser(id: UserId) {
  /* ... */
}

loadUser(asUserId('u_123')); // ok
loadUser('u_123'); // type error
```

Constructor functions (`asUserId`) are the only sanctioned place for the cast. Keep them next to the type and reuse them everywhere the primitive enters the domain.

### Exhaustiveness checking with `never`

Discriminated unions paired with an `assertNever` helper guarantee every case is handled at compile time.

```ts path=null start=null
type Event =
  | { kind: 'click'; x: number; y: number }
  | { kind: 'keypress'; key: string }
  | { kind: 'scroll'; delta: number };

function assertNever(value: never): never {
  throw new Error(`Unhandled variant: ${JSON.stringify(value)}`);
}

function describe(event: Event): string {
  switch (event.kind) {
    case 'click':
      return `click ${event.x},${event.y}`;
    case 'keypress':
      return `key ${event.key}`;
    case 'scroll':
      return `scroll ${event.delta}`;
    default:
      return assertNever(event);
  }
}
```

Adding a new variant to `Event` triggers a compile error at the `default` branch. Never use `default: return ...` without `assertNever` when switching on a discriminant.

### Zod and inferred types

Write the schema once; derive the TypeScript type from it. Never hand-write both.

```ts path=null start=null
import { z } from 'zod';

const User = z.object({
  id: z.string().uuid(),
  email: z.string().email(),
  role: z.enum(['admin', 'member', 'viewer']),
});

type User = z.infer<typeof User>;

export async function fetchUser(id: string): Promise<User> {
  const data = await api.get(`/users/${id}`);
  return User.parse(data); // throws on mismatch
}
```

Preserve the boundary: validate at every edge where untrusted data enters (HTTP requests, message queues, environment variables, third-party APIs, file reads). Once past the boundary, trust the static type.

Valibot is a lighter alternative with the same pattern (`v.InferOutput<typeof Schema>`). Pick one per project.

### `@ts-expect-error` discipline

- Prefer `@ts-expect-error` over `@ts-ignore`. It errors when the suppressed error disappears, keeping the suppression list accurate.
- Every `@ts-expect-error` must include a short justification and, ideally, a ticket reference: `// @ts-expect-error upstream types missing, fixed in #1234`.
- Never suppress type errors to ship; the suppression must be a conscious decision with a plan to remove it.
- Fail CI if the project's total `@ts-expect-error` count grows over the last merge to `main`.

### General TypeScript rules

- Prefer inference. Add explicit types at module boundaries (exported functions, public APIs) and let the rest infer.
- Model domain data with discriminated unions. Avoid `any`; use `unknown` with narrowing when genuinely uncertain.
- Derive types from single sources of truth (Zod, Valibot, Prisma, Drizzle). Never duplicate.
- `interface` for public object shapes consumers may extend; `type` for unions, intersections, mapped types, and internal aliases. Consistency matters more than the choice.
- `readonly` on fields and `ReadonlyArray<T>` on parameters that must not be mutated.
- Treat the type system as a design tool, not a syntax tax.

## JavaScript idioms

Conventions that apply equally to plain JavaScript and TypeScript. Formatter and linter take care of layout; these rules govern intent.

### Equality and truthiness

- `===` and `!==` always. `==` and `!=` are banned outside the single idiom `value == null` (matches `null` and `undefined`). Prefer explicit `value === null || value === undefined` when team reads diverge.
- Do not rely on implicit truthiness for numbers or strings that could legitimately be `0` or `""`. Write the explicit check: `if (count > 0)`, `if (name !== "")`.
- Boolean identifiers drop the `is` prefix: `visible`, `dirty`, `ready`, `enabled`. Event handlers that answer a predicate question may keep it (`shouldClose`).

### Async code

- `async`/`await` is the default. `.then()` chains are acceptable only for one-off transformations where a variable name would be noise.
- Never mix `await` inside a `.then()` callback. Pick one.
- `Promise.all` for independent parallel work; `Promise.allSettled` when one failure must not abort the others; `Promise.any` for first-success races.
- Always `await` or explicitly `void` a promise. Unhandled promise returns are a bug class the linter can catch (`no-floating-promises`).
- Cancel long-running async work with `AbortController`; pass the `signal` through to `fetch`, timers, and custom loops.

### Control flow

- Early return over nested `if`. Guard clauses at the top, happy path at the bottom.
- Prefer `switch` on discriminated unions with `assertNever`; prefer object lookup (`const handlers = { click: ..., scroll: ... }`) for simple dispatch.
- No `for`-loops when `for...of`, `map`, `filter`, `reduce`, or iterator helpers express the intent. Fall back to `for` only for hot-path numeric work where the profiler says so.

### Optional chaining and nullish coalescing

- `value?.x?.y` to read through potentially missing objects.
- `value ?? fallback` for nullish-only fallback. `||` only when falsy-zero or falsy-empty-string should also fall back (and document why).
- Chain both: `(config.retry ?? defaults.retry)?.strategy`.

### Event handlers and naming

- Functions that receive events: `handleClick`, `handleSubmit`, `handleKeydown`. Props that accept handlers: `onClick`, `onSubmit`, `onKeydown`.
- Do not inline complex handlers in JSX/template attributes. Extract to a named function once the body exceeds a single expression.

### Immutability

- Use `const` by default; `let` only when reassignment is required. Never `var`.
- Prefer non-mutating array methods (`map`, `filter`, `toSorted`, `toReversed`, `toSpliced`, `with`) in application code. In-place sorts and reverses in hot paths only, with a comment.
- Spread (`{ ...obj, key: value }`, `[...arr, item]`) for shallow copies. Use `structuredClone` for deep copies instead of JSON round-tripping.

### Error handling

- Always `throw new Error("...")` with a message that identifies the operation. Never `throw "string"`.
- Use `Error` subclasses or `cause` to preserve context: `throw new Error("failed to load user", { cause: err })`.
- Catch at boundaries (request handlers, job workers, CLI entry points). Inside business logic, let errors propagate unless there is a specific recovery.
- Reference `code-style-and-quality.md` for the broader error-handling taxonomy and the `Result` pattern option.

### Modules

- Named exports by default. Default exports only for single-component files where the filename already carries the name (React components, Astro pages).
- No `export *` barrels. They defeat tree-shaking and make import graphs opaque.
- Use path aliases (`@/components/button`) from the `tsconfig` `paths` setting. Never import via many `../../`.

## HTML code style

### Casing and quoting

- Lowercase element names and attribute names. SVG elements that must be camelCase (`viewBox`, `preserveAspectRatio`) keep their canonical casing.
- Double quotes around attribute values, always. Single quotes are reserved for attribute values that themselves contain double quotes.
- Boolean attributes appear without a value: `<input disabled>`, `<details open>`. Never `disabled="disabled"` or `disabled="true"`.
- Void elements (`<img>`, `<br>`, `<hr>`, `<input>`, `<meta>`, `<link>`) have no trailing slash in HTML. The self-closing form is only required inside JSX and XHTML.

### Attribute ordering

Consistent order inside a single tag makes scanning fast and diffs small:

1. `class`
2. `id`
3. `name`
4. `data-*` attributes
5. `src`, `href`, `for`, `type`, `value`
6. `alt`, `title`, `placeholder`
7. `aria-*` and `role`
8. `tabindex`
9. Event handlers (in HTML, avoid; use unobtrusive JS)
10. `style` (avoid; use classes)

```html path=null start=null
<a
  class="button button--primary"
  id="signup"
  data-test="cta"
  href="/signup"
  aria-label="Create an account">
  Sign up
</a>
```

### Indentation and wrapping

- Two-space indentation.
- Break long opening tags onto multiple lines when they exceed ~100 characters or carry more than three attributes. One attribute per line, closing `>` on the last attribute line (Prettier's `bracketSameLine: true`) — never floating below.
- Keep text content on the same line as the opening tag when it fits; break text out when the opening tag is multi-line.

### Vertical rhythm

Whitespace between elements is data for the reader, not the parser. The HTML spec has nothing to say about blank lines (the only normative guidance is that whitespace before `<html>` is dropped). The major style guides — Google, Idiomatic-HTML, Code Guide — say "use whitespace to improve readability" and stop there. Codify the rule so the source matches what Prettier preserves and what humans scan.

Insert exactly one blank line:

- Between top-level landmarks: `<header>` / `<main>` / `<footer>`, and between sibling `<section>`s inside `<main>`.
- Between sibling cards in a grid or list of card-shaped components (`.feature`, `.step`, `.stat`, `.testimonial`). The card boundary is the unit of meaning; let the eye land on it.
- Before an HTML comment that marks a structural region (`<!-- Hero -->`, `<!-- Features -->`).
- Between a multi-line block of `<script>` or `<style>` and the markup that follows.
- After the last element inside a long container, before the container's closing tag, only when the container exceeds ~30 lines and a visual break helps locate the boundary.

Do not insert blank lines:

- Inside tightly-coupled groups: short `<ul>`/`<ol>`, table rows, `<dt>`/`<dd>` pairs, dot-indicators, button groups, the children of a single card.
- Between inline siblings (`<a>`, `<span>`, `<code>`, `<em>`). Whitespace there is rendered output, not source rhythm — Prettier strips it under the default `htmlWhitespaceSensitivity: "css"`.
- Between an opening tag and its first child, or between the last child and the closing tag.
- More than one in a row. Prettier collapses runs to a single blank; the source should match.

Block sibling rhythm at a glance:

```html path=null start=null
<main>
  <!-- Hero -->
  <section class="hero">
    <h1>Headline</h1>
    <p class="lead">Lead paragraph.</p>
  </section>

  <!-- Features -->
  <section class="features">
    <div class="feature">
      <h3>One</h3>
      <p>Description.</p>
    </div>

    <div class="feature">
      <h3>Two</h3>
      <p>Description.</p>
    </div>

    <div class="feature">
      <h3>Three</h3>
      <p>Description.</p>
    </div>
  </section>
</main>
```

The same pattern, written without blank lines between sibling cards, reads as a wall of `<div>`. With them, the unit of meaning becomes scannable.

### Semantic first

- Use the element that carries the semantic: `<button>`, `<nav>`, `<main>`, `<article>`, `<header>`, `<footer>`, `<aside>`.
- Never build buttons out of `<div>` with a click handler. Accessibility, focus, keyboard support, and form integration are free on `<button>`.
- `<a>` for navigation (moves document, changes URL). `<button>` for in-page actions (opens modal, submits form, toggles state).
- Headings (`h1`–`h6`) follow document outline order. Do not skip levels for styling; restyle the element instead.

### Landmark and ARIA hygiene

- One `<main>` per page. Landmarks (`nav`, `header`, `footer`, `aside`, `section` with `aria-labelledby`) structure the page for assistive tech.
- Do not add `role="button"` to a `<button>`. Native roles are authoritative; ARIA is for elements that cannot be expressed natively.
- Label every interactive element: visible `<label for>` first, `aria-label` or `aria-labelledby` only when a visible label is impossible.

### Comments

- HTML comments (`<!-- -->`) for structural landmarks in long templates.
- Never ship conditional comments for IE.
- Remove commented-out markup. Version control remembers.

## Date and time discipline

Until Temporal is universally available:

- Store all timestamps in UTC (ISO 8601) on the server and in the database.
- Convert to the user's time zone at render time, not at storage time.
- Never store local time without a time-zone identifier.
- Use `Intl.DateTimeFormat` for locale-aware formatting; avoid hand-rolling formats.
- For durations and calendar math, adopt `Temporal` now behind a polyfill; it will age well.

## Keep the platform in view

When a new library promises to solve a problem, ask first whether the browser already can. Modern CSS, HTML, and JavaScript have eliminated entire library categories — state management for simple cases, animation for most interactions, date math, validation for forms, clipboard access, geolocation, lazy loading, dialogs, popovers, drag handles. Leaning on the platform produces code that loads faster, runs faster, and survives rewrites of its dependencies.
