# CSS and Layout

Modern CSS is not a subset of old CSS with extras. It is a genuinely different medium. Ignoring its capabilities produces brittle, media-query-heavy code that breaks on every unexpected viewport.

## Units: when to use which

- `rem` — typography, spacing, breakpoints, component sizing. Scales with the user's root-font preference.
- `em` — spacing that must scale with the local font size (button padding that grows with button text).
- `px` — hairline borders, small immovable UI details (1-px outlines, 2-px focus rings).
- `%` — dimensions relative to the parent where that relationship is genuinely desired.
- `ch` — reading widths (`max-inline-size: 65ch`), monospace columns, form input widths aligned to character count.
- `vh`, `vw` — layout that must scale with the viewport, but beware of mobile viewport behavior.
- `dvh`, `svh`, `lvh` (and `dvw`, `svw`, `lvw`) — dynamic, small, and large viewport units. `dvh` respects the browser chrome (toolbar) showing or hiding; `100dvh` is the "right" way to fill a mobile viewport.
- `cqi`, `cqb`, `cqw`, `cqh`, `cqmin`, `cqmax` — container query units. Scale with the containing element; invaluable for component-scoped sizing.
- `lh`, `rlh` — multiples of current and root line-height; excellent for vertical rhythm.

Rules:

- Never size body text in `px`. Users can override a root `rem` value for accessibility; `px` ignores that.
- Never use `vw` for font-size without `clamp()`. Raw `vw` text fails WCAG 1.4.4 (text resize).
- Never use `100vh` for full-height sections on mobile. Use `100dvh` with `100vh` fallback for older browsers.

## Responsive strategy

The goal is not "adapt at breakpoints." The goal is "work at every width." Breakpoints are a last resort, used where content genuinely demands a different structure.

Prefer intrinsic and fluid techniques in this order:

1. Natural flow — let content reflow; do not constrain it unnecessarily.
2. Flexbox with `flex-wrap: wrap` — wrapping components auto-adapt.
3. CSS Grid with `auto-fit` or `auto-fill` — column count adapts without media queries.
4. `clamp()` for fluid typography and spacing.
5. Container queries for component-level adaptation.
6. Media queries — for genuine page-level structural changes (e.g. sidebar hides on mobile).

The canonical breakpoint-free grid:

```css path=null start=null
.grid {
  --min-col: 18rem;
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(min(var(--min-col), 100%), 1fr));
  gap: clamp(1rem, 2vw, 2rem);
}
```

The `min(var(--min-col), 100%)` guard prevents overflow when the container is narrower than the desired minimum column width.

### Mobile-first vs desktop-first

Mobile-first is the default. Writing styles for narrow widths, then adding properties at larger widths, yields less code:

```css path=null start=null
.nav {
  display: flex;
  flex-direction: column;
}
@media (min-width: 48rem) {
  .nav {
    flex-direction: row;
  }
}
```

Desktop-first is acceptable for specific inversions (a complex desktop navigation that simplifies on mobile); each project picks one direction and stays consistent.

## Container queries

Container queries adapt a component to the size of its container, not the viewport. Required for reusable components that live in slots of varying width.

```css path=null start=null
.card {
  container: card / inline-size;
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 1rem;

  @container card (inline-size < 24rem) {
    grid-template-columns: 1fr;
  }
}
```

Notes and gotchas:

- A container cannot style itself inside its own container query. Move the container declaration to the parent when this is required.
- `container-type: inline-size` is the most common and avoids the problems associated with `size` (which requires the container to have a fixed block-size).
- Name containers (`container: card / inline-size`) as soon as nesting is likely. An anonymous container is only addressable by the nearest ancestor.
- Container units (`cqi`, `cqb`) let components scale their internal sizing continuously with the container. Combine with `clamp()` to prevent extremes.

Container queries do not replace media queries. Media queries handle device-level decisions (reduced motion, color scheme, screen orientation); container queries handle component decisions.

## Style queries

`@container style(--variant: primary)` allows styling children based on custom property values set on the container. Useful for theming variants without class plumbing.

## The `:has()` selector

`:has()` matches elements by descendants, siblings, or state. It removes whole classes of JavaScript once reserved for simple conditional styling.

```css path=null start=null
/* A card that changes when it contains a specific element */
.card:has(.card__badge) { padding-inline-start: 2rem; }

/* A form that signals validity */
form:has(:invalid) .submit { opacity: 0.6; }

/* A theme that reacts to a checkbox state */
body:has(#dark-toggle:checked) { color-scheme: dark; }
```

Browser support is now universal. Prefer `:has()` to JavaScript class toggling whenever style alone is needed.

## Cascade layers (`@layer`)

Cascade layers make specificity predictable. A declaration inside an earlier layer loses to one inside a later layer, regardless of selector specificity.

```css path=null start=null
@layer reset, base, components, utilities;

@layer reset { /* minimal normalizations */ }
@layer base { body { font: 1rem/1.5 system-ui; } }
@layer components { .button { ... } }
@layer utilities { .mt-4 { margin-top: 1rem; } }
```

Benefits:

- Third-party CSS can be loaded into a layer so it never outranks the application's styles.
- Utility CSS can be placed in a final layer so it always wins without needing `!important`.
- Debugging is easier: specificity becomes layer-driven and explicit.

Start every greenfield project with a named `@layer` stack.

## Native nesting

Nesting reduces repeated selectors and co-locates related rules. Supported in all modern browsers.

```css path=null start=null
.card {
  padding: 1rem;
  border-radius: 0.75rem;

  & > h2:first-child { margin-block-start: 0; }
  &:hover { box-shadow: var(--shadow-md); }
  @container (inline-size > 24rem) { padding: 1.5rem; }
}
```

Nesting is a convenience, not an architecture. Do not nest to mirror HTML structure, which multiplies specificity and reduces reusability. Keep nesting shallow.

## Subgrid

`subgrid` lets a child grid inherit tracks from its parent, enabling alignment across nested components.

```css path=null start=null
.cards {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: auto auto auto;
  gap: 1rem;
}
.card {
  display: grid;
  grid-row: span 3;
  grid-template-rows: subgrid;
}
```

Every card's title, body, and footer now align across the grid, even though the cards have different content lengths.

## `@scope`

`@scope` confines selectors to a portion of the DOM without leaking.

```css path=null start=null
@scope (.article) to (.comments) {
  p { max-inline-size: 65ch; }
}
```

Useful for content-management-system articles where downstream elements must escape the scope. Use sparingly; most component CSS is fine with regular nesting or layers.

## View transitions

View Transitions API animates the difference between two DOM states, single-page or multi-page.

Single-page:

```js path=null start=null
if (document.startViewTransition) {
  document.startViewTransition(() => updateDom());
} else {
  updateDom();
}
```

Multi-page:

```html path=null start=null
<meta name="view-transition" content="same-origin" />
```

Then assign a unique `view-transition-name` on elements that should tween between pages. The browser handles the cross-fade, morph, and scroll position automatically.

Respect reduced motion; disable transitions when `prefers-reduced-motion: reduce`.

## Scroll-driven animations

Animate properties based on scroll position without JavaScript.

```css path=null start=null
.reveal {
  animation: reveal linear;
  animation-timeline: view();
  animation-range: entry 0% cover 40%;
}
@keyframes reveal {
  from { opacity: 0; translate: 0 2rem; }
  to   { opacity: 1; translate: 0 0; }
}
```

Supported in Chromium; progressive enhancement elsewhere via polyfill or JavaScript Intersection Observer.

## `@property`, `color-mix()`, `light-dark()`, `@starting-style`, `attr()`

- `@property` registers a typed custom property. Required for animating gradients, `linear-gradient` color stops, and other non-animatable properties.
- `color-mix(in oklch, var(--brand) 50%, white)` composes colors in a perceptual space.
- `light-dark(var(--fg-light), var(--fg-dark))` selects per color scheme without extra selectors.
- `@starting-style` defines the initial state for entrance animations (useful with `popover` and `dialog`).
- `attr()` as a general-purpose value function supports typed values in modern browsers.

## `@function` and `if()`

`@function` declares custom CSS functions that return a single value. `if()` provides inline conditional values.

```css path=null start=null
@function --cols() {
  result: 1fr 1fr 1fr;
  @media (width < 500px) {
    result: 1fr;
  }
}

.grid {
  display: grid;
  grid-template-columns: --cols();
}
```

Browser support is currently Chromium-only; include fallbacks for safety.

## Logical properties

Always prefer logical properties on any project with even a hint of internationalization, and ideally on all projects:

- `margin-inline-start`, `margin-inline-end`, `margin-block-start`, `margin-block-end` instead of `left`, `right`, `top`, `bottom`.
- `padding-inline`, `padding-block` shorthands.
- `inset-inline`, `inset-block`.
- `inline-size`, `block-size` instead of `width`, `height`.
- `text-align: start` / `end` instead of `left` / `right`.

This is not theoretical: switching a site to RTL is trivial with logical properties and painful without.

## Motion

Good motion clarifies, bad motion decorates. Motion principles distilled from interaction design (Emil Kowalski and others):

- Origin and destination — motion should reveal where elements come from and where they go. An entering panel slides from its source edge; a popover grows from its anchor.
- Easing — ease-out is the default for entering motion; ease-in for exits; ease-in-out for movement between known positions. Avoid `linear` except for indeterminate progress.
- Spring physics — for interactions that should feel tactile (drag releases, pull-to-refresh), springs beat cubic Béziers.
- Duration — UI motion lives in 150–300 ms; longer readings a dramatic moment, shorter feels nervous.
- Interruptibility — user input during an animation must take precedence over finishing the animation.
- Staggering — grouped elements (list items, steps) animate with small offsets (30–50 ms each) so the eye follows the order.
- Respect `prefers-reduced-motion` — disable non-essential motion for users who have opted out.

```css path=null start=null
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Animate properties that stay on the compositor: `transform`, `opacity`, `filter`, `backdrop-filter`, `clip-path`. Avoid animating `width`, `height`, `margin`, `top`, `left`; they cause layout thrash.

For animating to `auto`, opt in with `interpolate-size: allow-keywords` on `html` or use `calc-size(auto, size)`:

```css path=null start=null
html { interpolate-size: allow-keywords; }

.accordion { height: 0; overflow: hidden; transition: height 0.2s ease-out; }
.accordion.open { height: auto; }
```

Use GSAP for timeline-based, orchestrated animations; use Motion (formerly Framer Motion) inside React projects that need spring physics and gesture handling; use plain CSS for single-property transitions.

### GSAP and ScrollTrigger

GSAP (GreenSock Animation Platform) is the industry standard for complex scripted animations. It is faster than CSS animations in many cases, handles transforms independently (rotate and scale with different timings, which CSS cannot), and irons out cross-browser inconsistencies. Over 11 million sites use it, including the majority of Awwwards winners.

Core concepts:

```js path=null start=null
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

// Tween: animate to a target state
gsap.to('.hero-text', { y: 0, opacity: 1, duration: 1.2, ease: 'power4.out' });

// Timeline: sequence multiple animations
const tl = gsap.timeline();
tl.from('.headline', { y: 100, opacity: 0, duration: 1.2, ease: 'power4.out' })
  .from('.cta', { y: 20, opacity: 0, duration: 1, ease: 'back.out(1.7)' }, '-=0.5');
```

ScrollTrigger ties animations to scroll position:

```js path=null start=null
// Animate on scroll entry
gsap.utils.toArray('.section').forEach(section => {
  gsap.from(section, {
    y: 80,
    opacity: 0,
    duration: 1,
    scrollTrigger: {
      trigger: section,
      start: 'top 85%',
      toggleActions: 'play none none reverse',
    },
  });
});

// Pin and scrub (animation progress tied to scroll position)
gsap.to('.image-reveal', {
  scale: 1,
  opacity: 1,
  ease: 'none',
  scrollTrigger: {
    trigger: '.image-section',
    start: 'top top',
    end: '+=600',
    pin: true,
    scrub: 0.5,
  },
});
```

Responsive and reduced-motion handling:

```js path=null start=null
// Respect reduced motion
if (!window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
  gsap.registerPlugin(ScrollTrigger);
  // ... animations
}

// Adapt animations per breakpoint
gsap.matchMedia({
  '(min-width: 768px)': function () {
    gsap.from('.sidebar', { x: -100, opacity: 0, duration: 1 });
  },
  '(max-width: 767px)': function () {
    gsap.from('.sidebar', { y: 50, opacity: 0, duration: 0.6 });
  },
});
```

When to use GSAP versus CSS:
- CSS transitions and `@keyframes`: single-property state changes, hover effects, simple entrances.
- CSS scroll-driven animations (`animation-timeline: view()`): simple scroll-linked effects where Chromium-only support is acceptable.
- GSAP + ScrollTrigger: complex timelines, pinning, scrubbing, staggered sequences, image sequences, cross-browser consistency required.
- Motion (Framer Motion): React projects with gesture-driven interactions and spring physics.

## Field sizing

`field-sizing: content` lets inputs and textareas grow with their content natively:

```css path=null start=null
textarea { field-sizing: content; min-block-size: 4lh; max-inline-size: 100%; }
```

Removes a common JavaScript responsibility; wider support arriving rapidly.

## Popover and anchor positioning

Native `popover` attribute plus anchor positioning remove the last genuine reasons for libraries that tether floating UI to elements:

```html path=null start=null
<button popovertarget="menu">Options</button>
<div id="menu" popover>
  <ul role="menu">…</ul>
</div>
```

```css path=null start=null
button { anchor-name: --menu-anchor; }
[popover] {
  position-anchor: --menu-anchor;
  top: anchor(bottom);
  left: anchor(start);
  margin-block-start: 0.5rem;
}
```

Still partially shipping across browsers; polyfills exist for both APIs.

## Forms and `:user-invalid`

Style validity without punishing users on first keystroke:

```css path=null start=null
input:user-invalid { border-color: var(--danger); }
input:user-valid   { border-color: var(--success); }
```

`:user-invalid` and `:user-valid` apply only after interaction, preventing red borders on a fresh form.

## A useful reset

A minimal, modern base that solves common annoyances without over-specifying:

```css path=null start=null
@layer reset {
  *, *::before, *::after { box-sizing: border-box; }
  html {
    color-scheme: light dark;
    font: clamp(1rem, 1rem + 0.25vw, 1.125rem) / 1.5 system-ui, sans-serif;
    interpolate-size: allow-keywords;
    -webkit-text-size-adjust: 100%;
  }
  body { margin: 0; min-block-size: 100dvh; }
  img, video, iframe, svg, picture {
    display: block; max-inline-size: 100%; block-size: auto;
  }
  h1, h2, h3, h4, h5, h6 { text-wrap: balance; }
  p, li, dd { text-wrap: pretty; max-inline-size: 70ch; }
  input, button, textarea, select { font: inherit; }
  :focus-visible { outline: 2px solid currentColor; outline-offset: 2px; }
  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
      animation-duration: 0.01ms !important;
      animation-iteration-count: 1 !important;
      transition-duration: 0.01ms !important;
      scroll-behavior: auto !important;
    }
  }
}
```

Put it in a `reset` layer and never think about `margin: 8px` on `body` again.
