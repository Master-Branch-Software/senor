# Accessibility

Accessibility is not a feature to add at the end; it is the baseline quality level of any usable interface. A site that cannot be operated without a mouse, seen with low vision, or understood by a screen reader is a broken site, regardless of how it looks.

## The shortest useful mental model

- Semantic HTML for everything possible.
- ARIA only when HTML cannot express the semantics.
- Keyboard access for every action.
- Visible focus for every focusable element.
- Color contrast that meets WCAG 2.2 AA.
- Motion that honors `prefers-reduced-motion`.
- Descriptive text for images, icons, and controls.

If each of these is true, most accessibility problems are already solved.

## WCAG 2.2 at a glance

WCAG 2.2 is the 2024-and-later standard; 2.1 is still referenced by some laws. Conformance has three levels (A, AA, AAA). AA is the practical target for most products and the legal threshold in many jurisdictions.

The four POUR principles:

- Perceivable — content can be perceived by more than one sense.
- Operable — the interface can be operated with keyboard, pointer, voice, or assistive technology.
- Understandable — content and operation are predictable and error-tolerant.
- Robust — content works with current and future user agents.

Notable success criteria to memorize:

- 1.1.1 Non-text content — images have equivalents in text.
- 1.3.1 Info and relationships — structure is conveyed programmatically (headings, labels, landmarks).
- 1.4.3 Contrast — 4.5:1 for text, 3:1 for large text.
- 1.4.4 Resize text — text can scale to 200% without loss.
- 1.4.10 Reflow — content reflows to a single column at 320 CSS pixels wide.
- 1.4.11 Non-text contrast — UI components and graphics meet 3:1.
- 2.1.1 Keyboard — all functionality is keyboard-operable.
- 2.1.2 No keyboard trap — focus can always move away.
- 2.4.3 Focus order — focus follows a meaningful order.
- 2.4.7 Focus visible — keyboard focus is always visible.
- 2.4.11 Focus not obscured — focused elements are not hidden by overlays (new in 2.2).
- 2.5.5 / 2.5.8 Target size — at least 24 × 24 CSS pixels (AA) with exceptions, 44 × 44 preferred.
- 3.2.1 / 3.2.2 On focus / on input — focus or input does not cause unexpected changes.
- 3.3.1 Error identification — errors are identified and described in text.
- 3.3.7 Redundant entry — previously entered information is auto-filled or available (new in 2.2).
- 4.1.2 Name, role, value — all UI components expose name, role, state, and value to assistive tech.

## Semantic HTML first

Use the platform. Most accessibility comes free with native elements:

- `<button>` — clickable action with built-in role, keyboard activation, and focus.
- `<a href>` — navigation with Enter activation.
- `<input>`, `<select>`, `<textarea>` — labeled, focusable, keyboard-operable form controls.
- `<dialog>` — modal with focus trap (with `showModal()`), `Escape` to close, and inert background (when opened modally).
- `<details>` / `<summary>` — accessible disclosure widget; no JavaScript.
- `<nav>`, `<main>`, `<header>`, `<footer>`, `<aside>`, `<section>`, `<article>` — landmark regions consumed by screen readers for navigation.
- `<figure>` / `<figcaption>` — images with captions; relationships preserved.
- `<table>`, `<th scope>`, `<caption>` — tabular data with programmatic headers.

Warning signs:

- `<div onClick>` or `<span onClick>` — missing role, missing keyboard, missing focus.
- `<a href="javascript:void(0)">` — hostile to assistive tech.
- `<div role="button">` — wrong answer. Use a `<button>`.
- Structured content inside `<div>` soup — screen readers cannot navigate.

## The five rules of ARIA

1. If a native HTML element can do it, use the native element. Do not redefine semantics that already exist.
2. Do not change native semantics unless absolutely necessary (`<h1 role="button">` is almost never right).
3. All interactive ARIA controls must be keyboard-operable. Implementing `role="button"` requires Enter/Space handling and focusability.
4. Do not use `role="presentation"` or `aria-hidden="true"` on a focusable element.
5. All interactive elements must have an accessible name (from text content, `aria-label`, `aria-labelledby`, or associated `<label>`).

Common useful ARIA attributes:

- `aria-label`, `aria-labelledby` — provide a name when one is not visible.
- `aria-describedby` — supplementary help or error text.
- `aria-expanded` — on a trigger for a collapsible region.
- `aria-controls` — links a trigger to the element it controls.
- `aria-current="page"` — marks the active nav item.
- `aria-live="polite"` / `"assertive"` — announces dynamic content changes.
- `aria-busy="true"` — indicates a region is being updated.
- `aria-invalid="true"` — marks a form field as failing validation.

Do not sprinkle ARIA defensively. Adding roles, states, or labels that duplicate or contradict native semantics is an accessibility regression.

## Accessible name

Every interactive element must have a programmatically determinable accessible name. Preferred sources, in order:

1. Visible text content (`<button>Save changes</button>`).
2. Associated `<label for>` (for form controls).
3. `aria-labelledby="id-of-label-element"` (uses existing visible text).
4. `aria-label="string"` (only when no visible label exists).
5. `title` (last resort; often ignored).

Icons with no text need a label: either `aria-label="Close"` on the button or visually hidden text inside it. SVG images can expose a title via `<title>` inside the SVG.

## Keyboard

All functionality must be reachable and operable by keyboard alone. Common expectations:

- Tab / Shift-Tab — move forward / backward through focusable elements.
- Enter — activate links and buttons; submit forms.
- Space — toggle checkboxes, radio buttons, switches; activate buttons.
- Arrow keys — move inside composite widgets (menus, radio groups, date pickers, sliders, tab lists).
- Escape — close modals, popovers, menus.
- Home / End — jump to the start or end of a list where appropriate.

Never set positive `tabindex` values. Only three values have a purpose:

- No `tabindex` — follows natural DOM order.
- `tabindex="0"` — make a non-focusable element focusable (almost never needed; use a `<button>` instead).
- `tabindex="-1"` — programmatically focusable but not in the tab order (for focus management of modals and sections).

### Focus management

Focus is a cursor for keyboard users. Treat it with the same care given to mouse position.

- When a modal or dialog opens, move focus into it (usually to the first focusable control or to the dialog heading).
- When it closes, return focus to the element that opened it.
- When a list item is deleted, move focus to the next item or the trigger.
- When the route changes in a single-page app, move focus to the new page's main heading or a skip-link target.
- Maintain visible focus; never suppress it with `outline: none` without an equivalent replacement.

### Focus trap in modals

Inside a modal, focus must cycle within the modal. `<dialog>` with `showModal()` handles this natively. For non-native modals:

- Collect all focusable descendants.
- On Tab from the last, move focus to the first.
- On Shift-Tab from the first, move focus to the last.
- Escape must close the modal.
- Use the `inert` attribute on siblings to remove them from the accessibility tree.

Libraries that do this correctly: React Aria, Radix UI, Reach UI, Headless UI, Chakra UI.

### Skip links

Every site needs a "Skip to main content" link as the first focusable element. Hide it visually until focused:

```html path=null start=null
<a href="#main" class="skip-link">Skip to main content</a>
```

```css path=null start=null
.skip-link {
  position: absolute;
  inset-inline-start: 0.5rem;
  inset-block-start: 0.5rem;
  padding: 0.5rem 0.75rem;
  background: var(--bg);
  color: var(--fg);
  clip-path: inset(50%);
}
.skip-link:focus-visible {
  clip-path: none;
}
```

## Focus styles

Focus styles fail when:

- `outline: none` is applied without a replacement.
- The outline has insufficient contrast against the background.
- The outline is thinner than 2 px, easily missed.
- Focus only appears on mouse click but not keyboard tab (use `:focus-visible`).

The default that rarely fails:

```css path=null start=null
:focus-visible {
  outline: 2px solid currentColor;
  outline-offset: 2px;
}
```

For a branded focus ring, use a color with at least 3:1 contrast against both the background and the element (WCAG 2.4.11/1.4.11).

## Forms

- Every `<input>`, `<select>`, `<textarea>` has a visible `<label>` associated via `for="id"` or by wrapping.
- Required fields are indicated both visually (asterisk with `aria-hidden="true"` plus text "required") and via `required`.
- Error messages are attached with `aria-describedby` and announced via `role="alert"` or a live region when they appear.
- Autocomplete attributes are set for known fields (`autocomplete="email"`, `"given-name"`, `"street-address"`), enabling password managers and faster input.
- Use the correct `input type` (`email`, `tel`, `url`, `date`, `number`) so mobile keyboards adapt.
- Do not disable copy/paste, autofill, or browser autocompletion.

## Images and media

- `alt` describes the image's purpose in context.
- Decorative images use `alt=""` (not omitted).
- Complex images (charts, diagrams) use `<figure>` with an extended description nearby or `aria-describedby` pointing to it.
- Videos have captions (for audio content) and transcripts (for screen readers). Audio content has transcripts.
- Autoplay is off by default; autoplay with sound is never acceptable.
- Animated media respects `prefers-reduced-motion`; animated GIFs should be replaced with paused-by-default `<video>` with a user-controlled play button.

## Zoom and reflow

- Test at 200% zoom. Content must remain usable without horizontal scrolling at a 320 CSS-pixel viewport width (WCAG 1.4.10).
- Test at 400% zoom. Content must still reflow without loss of information.
- Never disable user scaling in the viewport meta (`user-scalable=no`, `maximum-scale=1.0` are bugs).

The safe viewport meta:

```html path=null start=null
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

## Touch targets

Target size should be at least 24 × 24 CSS pixels for AA compliance, 44 × 44 for comfortable mobile use. When visual size is smaller, ensure spacing provides an adequate hit area around the element or use `@media (hover: none) and (pointer: coarse)` to upscale on touch devices.

## Motion and vestibular safety

Users with vestibular disorders can be nauseated or disoriented by certain motion. Large parallax, full-screen transitions, and perpetual movement are the worst offenders.

- Honor `prefers-reduced-motion` everywhere. Replace decorative animation with an instant cut or a cross-fade.
- Avoid auto-playing carousels; give the user pause controls.
- Limit parallax to small, subtle displacements.
- Never place text over a moving video or animated gradient without a motionless, contrast-safe background behind it.

## Color and contrast

- Text: 4.5:1 minimum, 7:1 for AAA.
- Large text (≥ 18 pt or 14 pt bold): 3:1 minimum.
- Graphical objects and UI components: 3:1 minimum.
- Focus indicators: 3:1 minimum against adjacent colors.
- Never use color alone to convey meaning. A red "error" state should include an icon and text.
- Test in grayscale and with common color-vision simulations (deuteranopia, protanopia, tritanopia).

## Live regions

For asynchronous updates users need to know about:

- `aria-live="polite"` — announced when the user is idle (toasts, success messages).
- `aria-live="assertive"` — announced immediately (critical errors, session timeouts); use sparingly.
- `role="status"` — polite live region, often used for loading.
- `role="alert"` — assertive live region, for errors.

Live regions must exist in the DOM before content is inserted; appending a live region and content simultaneously may not announce.

## Testing workflow

### Automated

Catches around 30–40% of issues. Still worth running on every build.

- axe DevTools (free browser extension; also available as `@axe-core/cli` for CI).
- Lighthouse accessibility audit (Chrome DevTools, Lighthouse CI).
- Accessibility Insights (Microsoft).
- Pa11y (CLI-based, good for integration tests).
- eslint-plugin-jsx-a11y (or framework-equivalent) for source-level checks.

### Manual, every release

- Tab through every page. Can the entire flow complete with keyboard alone? Is focus always visible?
- Press Escape in modals and popovers. Do they close and return focus correctly?
- Zoom to 200% and 400%. Is everything usable and does content reflow to one column at 320 CSS px?
- Test with at least one screen reader per major OS: VoiceOver (macOS/iOS), NVDA or JAWS (Windows), TalkBack (Android).
- Turn on `prefers-reduced-motion` (OS level). Does essential animation still convey meaning without motion? Is decorative motion disabled?
- Test in high-contrast mode (Windows) and forced-colors (`@media (forced-colors: active)`).

### Screen reader quick checklist

- Does every landmark make sense (one `<main>`, a `<nav>`, a `<header>`, a `<footer>`)?
- Is the heading structure a logical outline (`h1` then `h2`, not jumping from `h1` to `h4`)?
- Does each interactive element have a clear, descriptive name?
- Are dynamic updates announced?
- Does focus move logically after actions (submit, open modal, close modal, delete item)?

## Component library choices

Prefer libraries that handle accessibility by default:

- React Aria and React Aria Components (Adobe) — most rigorous accessibility implementation available.
- Radix UI Primitives — unstyled, accessible primitives; excellent default keyboard and focus behavior.
- Headless UI (Tailwind) — accessible primitives for React and Vue.
- Reach UI — accessible primitives for React.
- shadcn/ui — accessible components built on Radix, copied into the project source for full control.

Avoid rolling custom menus, dialogs, comboboxes, or date pickers. The ARIA Authoring Practices Guide has approximate patterns, but the details are easy to get wrong.

## What cannot be automated

Automated tools cannot tell whether:

- An `alt` text describes the image's purpose correctly.
- A heading hierarchy reflects content organization.
- A button label tells the user what will happen.
- A keyboard sequence is logical.
- A screen reader announcement is useful.

Budget time for manual review on every significant change. Treat accessibility as code quality, not as a gate at the end.
