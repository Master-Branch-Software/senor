# Response: pricing-page-a11y-contrast-fix (WITHOUT skill)

Here's how to make your pricing page accessible and fix the color contrast issues.

## Contrast Fixes

### 1. "Most Popular" Badge — White text on #FFA500

The white text on `#FFA500` (orange) is a common contrast failure. Orange is a mid-lightness color that doesn't have enough contrast against white.

**Fix**: Change the badge text to a dark color, or darken the background significantly.

Option A — Dark text on light orange:
```css path=null start=null
.badge {
  background-color: #FFA500;
  color: #1a1a1a; /* dark text on orange passes contrast */
}
```

Option B — Keep white text, darken the background:
```css path=null start=null
.badge {
  background-color: #cc7a00; /* darker orange */
  color: #ffffff;
}
```

### 2. "Current Plan" Button — White text on #CCCCCC

Light gray buttons with white text almost always fail contrast. This one definitely does.

**Fix**:
```css path=null start=null
.button-disabled {
  background-color: #767676; /* mid-gray with sufficient contrast against white */
  color: #ffffff;
  cursor: not-allowed;
  opacity: 0.8;
}
```

Or alternatively, use dark text on the light gray:
```css path=null start=null
.button-disabled {
  background-color: #CCCCCC;
  color: #3a3a3a; /* dark text on light gray */
  cursor: not-allowed;
}
```

---

## Comparison Table Accessibility

For the 12-feature comparison table, make sure it has proper semantic HTML:

```html path=null start=null
<table>
  <caption>Plan comparison</caption>
  <thead>
    <tr>
      <th scope="col">Feature</th>
      <th scope="col">Starter ($29/mo)</th>
      <th scope="col">Pro ($79/mo)</th>
      <th scope="col">Enterprise</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Feature name</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
    </tr>
    <!-- more rows -->
  </tbody>
</table>
```

Key things to add:
- `<caption>` element for screen readers
- `scope="col"` on header cells
- `scope="row"` if you have row headers

---

## General Accessibility Checklist

**Keyboard navigation**:
- Make sure users can Tab through all the pricing cards and CTAs
- Test that Enter/Space activates all buttons
- Check that the "Most Popular" card doesn't trap focus

**Font considerations**:
- Your serif heading font looks fine for accessibility as long as it's at least 18px and has adequate contrast
- Make sure body text is at least 16px
- Line height of 1.5 is recommended for body copy

**ARIA improvements**:
- Add `aria-label` to the "Most Popular" badge so screen readers understand it
- Mark disabled buttons with `aria-disabled="true"` and `tabindex="-1"` to remove them from tab order

**Color independence**:
- Don't rely solely on the orange badge color to indicate "most popular" — consider adding "(Recommended)" text or a star icon

---

## Quick Wins in Tailwind

Since you're using Tailwind, here are the relevant classes:

```html path=null start=null
<!-- Fixed badge -->
<span class="bg-amber-700 text-white text-sm font-semibold px-3 py-1 rounded-full">
  Most Popular
</span>

<!-- Fixed disabled button -->
<button
  disabled
  class="bg-gray-400 text-gray-800 cursor-not-allowed opacity-75 px-6 py-2 rounded"
>
  Current Plan
</button>
```

---

## Testing

After implementing, test with:
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/) — paste in your hex values
- Chrome DevTools Accessibility panel — shows contrast ratios inline
- Tab through the page manually to check keyboard flow
- Try with Windows High Contrast Mode enabled

Let me know if you want me to audit any specific component in more detail or help with the Tailwind implementation.
