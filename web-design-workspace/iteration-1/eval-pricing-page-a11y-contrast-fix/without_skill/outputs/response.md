Good call. The page has at least two obvious contrast problems:
- White text on `#FFA500` (Most Popular badge)
- White text on `#CCCCCC` (disabled Current Plan button)

## Quick fixes
1. Badge
   - Keep orange but darken it, for example `#C2410C`, and keep white text.
   - Or keep `#FFA500` and switch text to dark gray/black.
2. Disabled button
   - Use darker text on lighter gray, e.g. background `#E5E7EB`, text `#6B7280`.
3. Keep heading/body font pairing if you like the style, but ensure text size and contrast are readable.

## Tailwind example
```tsx
<span className="bg-orange-700 text-white px-2 py-1 rounded">
  Most Popular
</span>

<button
  disabled
  className="bg-gray-200 text-gray-500 cursor-not-allowed"
>
  Current Plan
</button>
```

This should make the pricing UI more readable without changing layout.
