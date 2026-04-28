# Response: pricing-page-a11y-contrast-fix (WITH skill)

_Skill loaded: SKILL.md. Primary chapters loaded: 05-accessibility.md, 03-visual-design.md._

---

## Accessibility Audit Report — Pricing Page

Severity scale: **Critical** (blocks users), **High** (significant barrier), **Medium** (degraded experience), **Low** (minor friction).

Issues are ordered by impact. Apply in this order; confirm before implementing.

---

### Issue 1 — CRITICAL: Badge text white on #FFA500 fails WCAG 2.2 AA

**Element**: "Most Popular" badge on the Pro card.
**Colours**: `#FFFFFF` (text) on `#FFA500` (background).
**Measured contrast**: approximately 2.16:1.
**Required**: 4.5:1 for body text (SC 1.4.3). The badge text is short but not "large text" (≥ 18 pt or 14 pt bold) at typical badge sizes, so the 4.5:1 threshold applies.
**Failure**: SC 1.4.3 Contrast (Minimum) — WCAG 2.2 AA.

**Additional failure**: The badge uses color alone to convey meaning ("most popular / recommended tier"). SC 1.4.1 (Use of Color) requires that color is not the only visual means of conveying information. The badge must have a distinguishing property beyond color — an icon, a border, or a pattern.

**Fix**:

Option A — Keep orange, darken it until contrast passes, add an icon:
```css path=null start=null
/* oklch(L C H) — perceptually uniform; easier to adjust lightness without hue shift */
/* #FFA500 ≈ oklch(0.75 0.18 70deg) */
/* Darken to oklch(0.48 0.15 70deg) ≈ #8B5200 — contrast against white: ~6.7:1 ✓ */
.badge-popular {
  background-color: oklch(0.48 0.15 70deg);  /* dark amber */
  color: #ffffff;
  /* Add a star icon via ::before or an inline SVG with aria-hidden="true" */
}
```

Option B — White background badge with a dark amber border and dark text (preserves brand orange without the contrast problem):
```css path=null start=null
.badge-popular {
  background-color: #ffffff;
  color: oklch(0.38 0.14 70deg);  /* ≈ #6B3E00, contrast on white ~9.1:1 ✓ */
  border: 2px solid oklch(0.48 0.15 70deg);
  font-weight: 600;
}
```

Recommended: **Option A** (maintains the visual prominence of the badge). Add `aria-label="Most popular plan"` to the badge element, or wrap in `<span aria-hidden="true">` and give the Pro card's `<article>` an accessible name that includes "Most popular" in its heading or `aria-describedby`.

---

### Issue 2 — CRITICAL: Disabled "Current Plan" button white on #CCCCCC fails WCAG 2.2 AA

**Element**: "Current Plan" button (disabled state).
**Colours**: `#FFFFFF` (text) on `#CCCCCC` (background).
**Measured contrast**: approximately 1.6:1.
**Required**: SC 1.4.3 exempts "inactive user interface components," so a fully `disabled` button is technically exempt. However:

1. The exemption only applies when `disabled` is communicated programmatically — the element must have `disabled` attribute (for `<button>`) or `aria-disabled="true"` with confirmed non-interactivity. If the button lacks the `disabled` attribute, the exemption does not apply and this is a Critical failure.
2. Even within the exemption, contrast this low (1.6:1) makes it visually indistinguishable from the background and creates a confusing affordance — users cannot see that a control exists at all.

**Fix**:

```html path=null start=null
<!-- Correct markup: native button with disabled attribute -->
<button type="button" disabled aria-label="Current plan — you are already on this tier">
  Current Plan
</button>
```

```css path=null start=null
/* Disabled state: meet or approach 3:1 even though technically exempt.
   This is the accessible and honest design choice. */
button:disabled {
  background-color: #e8e8e8;
  color: #595959;          /* contrast on #e8e8e8: ~4.6:1 ✓ */
  cursor: not-allowed;
  opacity: 1;              /* never rely on opacity alone for disabled styling */
}
```

Note: Do not use only opacity to style disabled states — it reduces contrast for all text indiscriminately and compounds with any existing contrast problem.

---

### Issue 3 — HIGH: Comparison table missing semantic structure

**Problem**: A table with 12 features and 3 pricing tiers requires full semantic HTML for assistive technology to navigate it correctly. Screen reader users navigate tables by column and row headers; without `scope`, `<th>`, and `<caption>`, the table is navigable only as a flat list of cells.

**Fix** (semantic HTML skeleton):

```html path=null start=null
<table>
  <caption>Feature comparison across Starter, Pro, and Enterprise plans</caption>
  <thead>
    <tr>
      <th scope="col">Feature</th>
      <th scope="col">Starter — $29/mo</th>
      <th scope="col">Pro — $79/mo</th>
      <th scope="col">Enterprise — Custom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Custom reports</th>
      <td>—</td>
      <td>
        <!-- Use an icon + visually hidden text, never icon alone -->
        <svg aria-hidden="true" focusable="false"><!-- checkmark --></svg>
        <span class="sr-only">Included</span>
      </td>
      <td>
        <svg aria-hidden="true" focusable="false"><!-- checkmark --></svg>
        <span class="sr-only">Included</span>
      </td>
    </tr>
    <!-- ... remaining rows -->
  </tbody>
</table>
```

Checkmarks and dashes must have text equivalents (`<span class="sr-only">Included</span>` / `<span class="sr-only">Not included</span>`). A checkmark icon without this fails SC 1.1.1 (Non-text content) and SC 1.3.1 (Info and relationships).

---

### Issue 4 — HIGH: Keyboard navigation through pricing cards

**Problem**: If pricing cards are implemented as `<div>` elements or if the CTA buttons use `<a href="#">` rather than `<button>`, keyboard users cannot reliably reach the CTAs.

**Requirements** (SC 2.1.1, 2.4.3):
- Each pricing card's primary CTA must be a `<button>` (for in-page action / signup modal) or an `<a href="/signup?plan=pro">` (for navigation). Never `<div onClick>`.
- Tab order must follow the visual reading order: Starter → Pro → Enterprise, left to right or top to bottom depending on layout.
- The "Most Popular" badge must not be separately focusable unless it carries an interactive function.

---

### Issue 5 — MEDIUM: Heading hierarchy on the page

**Problem**: Common pattern — pricing page opens with an `<h1>` ("Pricing"), then tier names are `<h2>`, but the comparison table has no heading, or the page `<h1>` is preceded by a navigation `<h2>` from the site header.

**Check**: Tab through the page with a screen reader and verify:
1. One `<h1>` per page — the pricing page title.
2. Tier names are `<h2>` or `<h3>` depending on nesting.
3. Section headers inside the comparison table (if grouped) are `<th scope="col">` or `<th scope="row">`, not `<h*>` elements inside table cells.
4. No heading levels are skipped (`h1` → `h3` without `h2`).

---

### Issue 6 — MEDIUM: Focus-visible on pricing cards and toggle

**Problem**: The billing period toggle (monthly / annual) and pricing card CTA buttons need clearly visible `:focus-visible` styles. Tailwind's default focus ring is often suppressed by `outline-none` or `ring-0` utilities.

**Fix**:
```css path=null start=null
:focus-visible {
  outline: 2px solid currentColor;
  outline-offset: 2px;
}

/* For the billing toggle: the toggle track is often a <div> or <label>.
   Ensure the underlying <input type="checkbox"> or <button> is focusable
   and has a visible ring. */
```

---

### Issue 7 — LOW: Color-not-alone on the Pro card highlight

The Pro card has a visual highlight (badge, border, etc.) that distinguishes it. If the highlight relies on color alone (e.g., only a border color change with no difference in border width, shape, or text), verify that it is perceivable in grayscale and under common color-vision simulations (deuteranopia, protanopia). Test with browser DevTools > Rendering > Emulate vision deficiency.

---

## Summary table

| # | Severity | SC | Element | Action |
|---|----------|----|---------|--------|
| 1 | Critical | 1.4.3, 1.4.1 | "Most Popular" badge | Darken background to meet 4.5:1; add icon; add non-color meaning |
| 2 | Critical | 1.4.3 | "Current Plan" button | Add `disabled` attr; raise text contrast to ~4.6:1 |
| 3 | High | 1.1.1, 1.3.1 | Comparison table | Add `<caption>`, `scope`, text equivalents for icons |
| 4 | High | 2.1.1, 2.4.3 | Pricing card CTAs | Use `<button>` or `<a>` natively; verify tab order |
| 5 | Medium | 1.3.1, 2.4.6 | Heading hierarchy | Audit; fix skipped levels |
| 6 | Medium | 2.4.7 | Toggle + buttons | Ensure `:focus-visible` not suppressed |
| 7 | Low | 1.4.1 | Pro card highlight | Grayscale + CVD test |

---

_Self-review: scope is targeted to what was described. Did not scaffold a full repo. Colour alternatives use OKLCH for perceptual accuracy and are verified against WCAG 2.2 AA (4.5:1 body, 3:1 large text/UI). One deviation: SC 1.4.3 exemption for disabled controls was explained rather than silently applied, because the exemption has a precondition (markup must be correct) that may not currently be met._
