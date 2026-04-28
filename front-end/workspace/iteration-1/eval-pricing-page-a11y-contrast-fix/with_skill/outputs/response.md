Targeted accessibility + contrast fixes only (no repo scaffolding).

## 1) Contrast failures identified
- `#FFFFFF` text on `#FFA500` badge background fails WCAG AA for normal text (too low for 4.5:1).
- `#FFFFFF` text on `#CCCCCC` disabled button background also fails (well below 4.5:1).

## 2) Color fixes with concrete replacements
Use either of these compliant patterns:

### Most Popular badge
- **Option A (keep white text):** `bg-[#9A3412] text-white` (dark orange + white)
- **Option B (keep bright orange):** `bg-[#FFA500] text-[#1F2937]`

### Disabled Current Plan button
- `bg-[#E5E7EB] text-[#4B5563] border border-[#9CA3AF]`
- Keep disabled semantics: `disabled` attribute + `aria-disabled="true"` when needed by component abstraction.

These combinations satisfy WCAG 2.2 AA expectations for text contrast; also keep UI/focus indicators at least 3:1.

## 3) Pricing table semantics (12-feature comparison)
Use real table structure, not div grid:
- `<table>`
- `<caption>` describing what is compared
- `<thead>` with column headers (`<th scope="col">`)
- `<tbody>` rows where first cell is row header (`<th scope="row">Feature</th>`)
- Keep plan names and prices as real text, not image text

## 4) Keyboard + focus behavior
- Every interactive pricing control must be reachable by keyboard in logical order.
- Add strong `:focus-visible` treatment (min 2px ring, contrast >= 3:1).
- If Pro card is visually emphasized, do not alter tab order (`tabindex` stays natural; never positive).
- If card selection opens details, move focus predictably to revealed content heading.

## 5) Tailwind-oriented patch example
```tsx
<span className="inline-flex rounded-full bg-[#9A3412] px-2.5 py-1 text-xs font-semibold text-white">
  Most Popular
</span>

<button
  type="button"
  disabled
  className="rounded-md border border-[#9CA3AF] bg-[#E5E7EB] px-4 py-2 text-[#4B5563] focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-[#1D4ED8] disabled:cursor-not-allowed"
>
  Current Plan
</button>
```

If useful, I can provide a compact diff for a typical Next.js + Tailwind pricing table component next.
