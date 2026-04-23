You have a few clear performance issues:

1. Sorting in `useEffect` + state update
   - Every time `products` or `sort` changes, you sort and then trigger another render via `setFiltered`.
2. Inline callback in list
   - `onAddToCart={() => addToCart(p)}` creates a new function per item on each render.
3. Large list size
   - 500 cards can be heavy on mobile.

## Improvements
- Use `useMemo` for sorted products instead of storing derived state.
- Use `useCallback` for add-to-cart handlers.
- Consider list virtualization (`react-window` or `react-virtualized`).

## Example
```tsx
export function ProductGrid({ products }) {
  const [sort, setSort] = useState("price");

  const filtered = useMemo(() => {
    return [...products].sort((a, b) => a[sort] - b[sort]);
  }, [products, sort]);

  const handleAddToCart = useCallback((product) => {
    addToCart(product);
  }, []);

  return (
    <div className="grid grid-cols-3 gap-4">
      {filtered.map((p) => (
        <ProductCard
          key={p.id}
          product={p}
          onAddToCart={() => handleAddToCart(p)}
        />
      ))}
    </div>
  );
}
```

This should reduce work per render and improve responsiveness.
