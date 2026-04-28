High-impact issues first (mobile + 500 items + parent re-render every 2s):

## 1) Remove avoidable work on every render
Current anti-pattern: derived state via `useEffect` (`sorted -> setFiltered`) causes extra render cycles.
Use `useMemo` and render directly from memoized data.

## 2) Stop prop churn into each `ProductCard`
`onAddToCart={() => addToCart(p)}` creates a new function for every card on every parent render, which defeats memoization.
Use a stable callback and pass an identifier/value from the child.

## 3) Virtualize the grid
At 500 items, windowing is usually the biggest win on mobile (`@tanstack/react-virtual` or `react-window`).

## 4) Make layout responsive
`grid-cols-3` at all widths hurts small screens and can increase layout/paint cost.
Use responsive breakpoints (1/2/3 columns).

## 5) Measure impact with web-vitals
Track INP (interaction latency) and CLS, plus React Profiler commit time before/after.

---

### Refactor sketch (ordered for quick adoption)
```tsx
import { memo, useCallback, useMemo, useState } from "react";

const ProductCard = memo(function ProductCard({ product, onAddToCart }) {
  return (
    <article>
      <h3>{product.name}</h3>
      <button onClick={() => onAddToCart(product.id)}>Add to cart</button>
    </article>
  );
});

export function ProductGrid({ products, addToCart }) {
  const [sort, setSort] = useState("price");

  const sortedProducts = useMemo(() => {
    return [...products].sort((a, b) => a[sort] - b[sort]);
  }, [products, sort]);

  const handleAddToCart = useCallback(
    (productId) => {
      const product = sortedProducts.find((p) => p.id === productId);
      if (!product) return;
      addToCart(product);
    },
    [addToCart, sortedProducts]
  );

  return (
    <div className="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-3">
      {sortedProducts.map((p) => (
        <ProductCard key={p.id} product={p} onAddToCart={handleAddToCart} />
      ))}
    </div>
  );
}
```

### Additional notes
- If sort key is always numeric (`price`, `rating`), type it narrowly to avoid dynamic key penalties.
- For virtualized rendering, keep item height predictable or use measured row virtualization.
- If polling updates product objects every 2s, preserve referential stability where possible (update only changed items) to reduce list churn.

### Measurement checklist
- INP p75 before/after on product grid interactions
- CLS p75 (ensure responsive changes do not shift controls unexpectedly)
- React Profiler: commit duration and render count per `ProductCard`
- CPU throttled mobile profile (4x slowdown) for scroll + add-to-cart path
