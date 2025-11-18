# Patterns & Conventions

**📅 Documented:** November 18, 2025
**🔬 Tech Stack:** [View Research](TECH_STACK_RESEARCH.md)
**👥 Audience:** All developers
**⏱️ Estimated Reading:** 20-25 minutes

---

## Code Organization

### File Structure

```
src/
├── pages/
│   ├── products/
│   │   ├── list.tsx
│   │   ├── create.tsx
│   │   ├── edit.tsx
│   │   └── show.tsx
├── components/
│   ├── product/
│   │   ├── ProductCard.tsx
│   │   └── ProductFilter.tsx
├── hooks/
│   └── useProductStats.ts
├── providers/
│   ├── dataProvider.ts
│   └── authProvider.ts
└── types/
    └── product.ts
```

---

## Naming Conventions

### Components

```tsx
// ✅ PascalCase for components
ProductList.tsx
ProductCreate.tsx
ProductEditForm.tsx

// ❌ Avoid
productList.tsx
product-list.tsx
```

### Hooks

```tsx
// ✅ camelCase, start with "use"
useProductStats.ts
useCategories.ts

// ❌ Avoid
ProductStats.ts
getProductStats.ts
```

### Types

```tsx
// ✅ PascalCase, prefix with I for interfaces
interface IProduct {
  id: number;
  name: string;
}

type ProductStatus = "active" | "inactive";
```

---

## TypeScript Patterns

### Define Types

```typescript
// types/product.ts
export interface IProduct {
  id: number;
  name: string;
  price: number;
  categoryId: number;
  createdAt: string;
}

export interface ICategory {
  id: number;
  name: string;
}
```

### Use Generics

```tsx
const { data } = useList<IProduct>({
  resource: "products",
});
// data is typed as GetListResponse<IProduct>
```

---

## Hook Patterns

### Composing Hooks

```tsx
// ✅ Good - compose hooks
function ProductEdit() {
  const { id } = useResource();
  const { data: product } = useOne<IProduct>({ resource: "products", id });
  const { selectProps } = useSelect({ resource: "categories" });
  const { formProps } = useForm();

  return <Form {...formProps} />;
}
```

### Custom Hooks

```tsx
// hooks/useProductStats.ts
export const useProductStats = () => {
  const { data: products } = useList<IProduct>({ resource: "products" });

  const stats = useMemo(() => ({
    total: products?.total ?? 0,
    avgPrice: /* calculate */,
  }), [products]);

  return stats;
};
```

---

## Error Handling

### Try-Catch in Mutations

```tsx
const { mutate } = useCreate();

const handleCreate = async (values) => {
  try {
    await mutate({
      resource: "products",
      values,
    });
    message.success("Created!");
  } catch (error) {
    message.error(error.message);
  }
};
```

### Error Boundaries

```tsx
<ErrorBoundary>
  <ProductList />
</ErrorBoundary>
```

---

## Performance Patterns

### Memoization

```tsx
const columns = useMemo(() => [
  { field: "id", headerName: "ID" },
  { field: "name", headerName: "Name" },
], []);
```

### Conditional Fetching

```tsx
const { data } = useMany({
  resource: "categories",
  ids: categoryIds,
  queryOptions: {
    enabled: categoryIds.length > 0,  // Only fetch if IDs exist
  },
});
```

---

## Summary

**Key Patterns:**
- PascalCase components
- camelCase hooks
- TypeScript everywhere
- Compose hooks
- Handle errors
- Optimize performance

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
