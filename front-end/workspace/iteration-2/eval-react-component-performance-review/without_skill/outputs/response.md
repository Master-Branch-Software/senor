# Response: react-component-performance-review (WITHOUT skill)

Here's a review of the `ProductGrid` component with the performance issues I can spot:

## Issues Found

### 1. useEffect + setState for sorting (re-renders on every parent render)

The `useEffect` watching `[products, sort]` sets state on every change. Since the parent re-renders every 2 seconds due to polling, and likely creates a new `products` array reference each time, this triggers a sort + setState every 2 seconds, causing the component to re-render constantly.

**Fix**: Replace with `useMemo`:

```tsx path=null start=null
// Before
const [filtered, setFiltered] = useState(products);
useEffect(() => {
  const sorted = [...products].sort((a, b) => a[sort] - b[sort]);
  setFiltered(sorted);
}, [products, sort]);

// After
const filtered = useMemo(
  () => [...products].sort((a, b) => a[sort] - b[sort]),
  [products, sort]
);
```

This avoids the extra render cycle.

### 2. Inline arrow function creating new reference on every render

```tsx path=null start=null
onAddToCart={() => addToCart(p)}
```

This creates a new function on every render, which means if `ProductCard` uses `React.memo`, it will still re-render because the prop changes every time.

**Fix**: Use `useCallback`:

```tsx path=null start=null
const handleAddToCart = useCallback((product) => {
  addToCart(product);
}, [addToCart]);

// In JSX:
<ProductCard
  key={p.id}
  product={p}
  onAddToCart={() => handleAddToCart(p)}
/>
```

Wait — actually the handler still needs `p` so you need to either pass `p` as an argument from the child, or create one memoized handler per item using `useCallback` in `ProductCard`:

```tsx path=null start=null
// Better: ProductCard calls it with the product
// ProductGrid:
const handleAddToCart = useCallback((product) => {
  addToCart(product);
}, [addToCart]);

// ProductCard internally:
<button onClick={() => onAddToCart(product)}>Add to cart</button>
```

### 3. No React.memo on ProductCard

Since the parent re-renders frequently, `ProductCard` should be wrapped in `React.memo` to prevent unnecessary re-renders when its props haven't changed:

```tsx path=null start=null
export const ProductCard = React.memo(({ product, onAddToCart }) => {
  // ...
});
```

### 4. No virtualization for 500 items

Rendering 500 product cards to the DOM is expensive, especially on mobile. You should virtualize the list so only the visible items are rendered.

Options:
- `react-window` — lightweight, simple API
- `react-virtual` (@tanstack/virtual) — more flexible
- `react-virtuoso` — easiest to use with variable-height items

Example with react-window:

```tsx path=null start=null
import { FixedSizeGrid } from 'react-window';

// Wrap your grid in a FixedSizeGrid
<FixedSizeGrid
  columnCount={3}
  columnWidth={300}
  height={600}
  rowCount={Math.ceil(filtered.length / 3)}
  rowHeight={400}
  width={900}
>
  {({ columnIndex, rowIndex, style }) => {
    const index = rowIndex * 3 + columnIndex;
    const product = filtered[index];
    if (!product) return null;
    return (
      <div style={style}>
        <ProductCard product={product} onAddToCart={handleAddToCart} />
      </div>
    );
  }}
</FixedSizeGrid>
```

### 5. Missing responsive layout

```tsx path=null start=null
<div className="grid grid-cols-3 gap-4">
```

This doesn't adapt to mobile screens. On small screens, 3 columns will be very cramped.

**Fix**:
```tsx path=null start=null
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
```

---

## Priority order

1. **useMemo** instead of useEffect — eliminates the biggest re-render loop
2. **React.memo + useCallback** — prevents child re-renders
3. **Virtualization** — most impactful for mobile performance with 500 items
4. **Responsive grid** — fixes mobile layout

---

## Quick summary of what the fixed component looks like

```tsx path=null start=null
export function ProductGrid({ products }) {
  const [sort, setSort] = useState('price');

  const filtered = useMemo(
    () => [...products].sort((a, b) => a[sort] - b[sort]),
    [products, sort]
  );

  const handleAddToCart = useCallback((product) => {
    addToCart(product);
  }, []);

  return (
    <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
      {filtered.map((p) => (
        <ProductCard
          key={p.id}
          product={p}
          onAddToCart={handleAddToCart}
        />
      ))}
    </div>
  );
}
```

For 500 items, also add virtualization — that alone will make the biggest difference on mobile.
