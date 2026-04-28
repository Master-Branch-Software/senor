# Response: react-component-performance-review (WITH skill)

_Skill loaded: SKILL.md. Primary chapters loaded: 06-performance.md, 07-javascript-and-html.md, 09-anti-patterns-and-process.md._

---

## Performance Review — ProductGrid

Issues are ordered by impact. Each has a concrete fix; measure INP and re-render counts before and after with React DevTools Profiler and `web-vitals`.

---

### Issue 1 — HIGH IMPACT: `products` reference triggers re-sort every 2 seconds

```tsx path=null start=null
useEffect(() => {
  const sorted = [...products].sort((a, b) => a[sort] - b[sort]);
  setFiltered(sorted);
}, [products, sort]);
```

**Problem**: The parent re-renders every 2 seconds (polling hook). If `products` is a new array reference on each parent render — even if the data is identical — this `useEffect` fires every 2 seconds, calling `setFiltered`, which triggers a full re-render of `ProductGrid` with 500 items.

This is the primary driver of the sluggishness. Two compounding sub-problems:
1. `useEffect` for derived state is the canonical React anti-pattern (09-anti-patterns-and-process.md). Sorting is pure derivation, not a side effect.
2. `[...products].sort()` on 500 items every 2 seconds is CPU work on the main thread, directly hurting INP (the 50 ms long-task threshold from 06-performance.md).

**Fix**: Replace `useEffect` + `useState` with `useMemo`:

```tsx path=null start=null
const filtered = useMemo(
  () => [...products].sort((a, b) => a[sort] - b[sort]),
  [products, sort]
);
```

This still re-sorts when `products` or `sort` changes, but it does not go through `setState` → re-render. If `products` is a new reference every 2 seconds despite identical data, stabilize it in the parent with `useMemo` or ensure the polling hook returns a stable reference when the data is unchanged (compare by value, not reference).

---

### Issue 2 — HIGH IMPACT: Inline arrow function defeats memoization on every render

```tsx path=null start=null
onAddToCart={() => addToCart(p)}
```

**Problem**: A new function reference is created for every `ProductCard` on every `ProductGrid` render. If `ProductCard` is wrapped in `React.memo`, the memo is useless because the prop reference always changes. With 500 items and a parent polling every 2 seconds, this is 500 new function allocations every 2 seconds plus 500 unnecessary re-renders if `React.memo` is not used.

**Fix**: Wrap the handler in `useCallback`:

```tsx path=null start=null
const handleAddToCart = useCallback(
  (product: Product) => addToCart(product),
  [addToCart]  // addToCart must itself be stable; memoize it at its origin if not
);

// Then in the render:
<ProductCard
  key={p.id}
  product={p}
  onAddToCart={handleAddToCart}
/>
```

And memoize `ProductCard`:

```tsx path=null start=null
export const ProductCard = React.memo(function ProductCard({ product, onAddToCart }) {
  // ...
});
```

`React.memo` + `useCallback` together prevent the cascade. Neither works without the other here.

---

### Issue 3 — HIGH IMPACT: 500 DOM nodes without virtualization

**Problem**: Rendering 500 `ProductCard` elements is 500 DOM nodes in the initial paint and on every re-render. On mobile, layout, style recalculation, and paint cost scales with DOM size. This directly hurts INP (any interaction requires the browser to work through a large DOM) and contributes to CLS risk if cards load content async.

**Fix**: Virtualize the list. Only render the cards currently in the viewport.

```tsx path=null start=null
import { useVirtualizer } from '@tanstack/react-virtual';

function ProductGrid({ products }) {
  const filtered = useMemo(
    () => [...products].sort((a, b) => a[sort] - b[sort]),
    [products, sort]
  );

  const parentRef = useRef<HTMLDivElement>(null);

  const rowVirtualizer = useVirtualizer({
    count: filtered.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 300,  // estimated card height in px
    overscan: 5,
  });

  return (
    <div ref={parentRef} style={{ height: '800px', overflow: 'auto' }}>
      <div style={{ height: rowVirtualizer.getTotalSize() }}>
        {rowVirtualizer.getVirtualItems().map((virtualRow) => (
          <div
            key={virtualRow.index}
            style={{
              position: 'absolute',
              top: virtualRow.start,
              width: '100%',
            }}
          >
            <ProductCard
              product={filtered[virtualRow.index]}
              onAddToCart={handleAddToCart}
            />
          </div>
        ))}
      </div>
    </div>
  );
}
```

`@tanstack/virtual` (or `react-window` if you prefer) means only ~10–20 cards are in the DOM at any time instead of 500.

---

### Issue 4 — MEDIUM IMPACT: `grid-cols-3` has no responsive breakpoints

```tsx path=null start=null
<div className="grid grid-cols-3 gap-4">
```

**Problem**: Three columns on mobile (320 px viewport) produces ~100 px-wide cards, which are unreadable and untappable. WCAG 2.5.8 requires 24 × 24 px minimum touch targets; realistically 44 × 44 px for comfortable use. This also contributes to CLS if images in cards have no reserved dimensions.

**Fix**:

```tsx path=null start=null
<div className="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-3">
```

Or use `auto-fit` for fully fluid columns that don't need breakpoint management:

```tsx path=null start=null
<div className="grid gap-4" style={{ gridTemplateColumns: 'repeat(auto-fit, minmax(min(100%, 16rem), 1fr))' }}>
```

---

### Issue 5 — MEDIUM IMPACT: INP and CLS considerations

**INP**: After fixing issues 1–3, profile with `web-vitals/attribution` to verify the 2-second re-render is no longer the top INP contributor. If `addToCart` itself is expensive (network call, heavy state update), wrap it in `startTransition` so it does not block the frame:

```tsx path=null start=null
import { startTransition } from 'react';

const handleAddToCart = useCallback((product) => {
  startTransition(() => addToCart(product));
}, [addToCart]);
```

**CLS**: Each `ProductCard` should have reserved dimensions. If cards contain images, those images must have `width` and `height` attributes (or an `aspect-ratio` CSS rule) so the browser reserves space before the image loads, preventing layout shift on arrival.

---

## Recommended fix order

1. **Replace `useEffect`/`useState` with `useMemo`** — eliminates the 2-second setState cycle entirely. Highest ROI, one line.
2. **Add `useCallback` + `React.memo`** — stops the 500-function-per-render allocation. Two additions.
3. **Add virtualization** — reduces DOM to ~20 nodes. Most work; highest ceiling benefit on mobile.
4. **Fix responsive grid** — removes mobile layout breakage.
5. **Measure** before calling done. Use React DevTools Profiler to confirm re-render count drops; use `web-vitals` to confirm INP improves in the field.

---

_Self-review: scope is the component as described. Did not rewrite the entire app or add unnecessary dependencies — only `@tanstack/virtual` which solves a specific, measured problem (500 DOM nodes). INP and CLS considerations noted per chapter 06. Anti-patterns (useEffect for derived state, inline handlers) addressed per chapter 09._
