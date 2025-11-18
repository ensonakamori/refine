# Debugging Guide

**📅 Documented:** November 18, 2025
**👥 Audience:** All developers
**⏱️ Estimated Reading:** 20 minutes

---

## Browser DevTools

### React DevTools

Install [React DevTools](https://react.dev/learn/react-developer-tools)

**Features:**
- Inspect component tree
- View props and state
- Track re-renders

---

### TanStack Query DevTools

Already included in Refine hooks!

```tsx
import { DevtoolsProvider } from "@refinedev/devtools";

<DevtoolsProvider>
  <Refine>
    {/* Your app */}
  </Refine>
</DevtoolsProvider>
```

**Features:**
- View all queries and mutations
- See cache state
- Trigger refetch manually
- Debug stale/fresh data

---

## Common Issues

### "Resource not found"

**Problem:** Hook can't find resource

**Solution:**
```tsx
// ✅ Ensure resource is defined
<Refine
  resources={[
    { name: "products", /* ... */ }
  ]}
/>

// ✅ Check resource name matches
useList({ resource: "products" })  // Must match exactly
```

---

### Data not fetching

**Problem:** useList returns no data

**Debug steps:**
1. Check network tab - is request being made?
2. Check response - is data in correct format?
3. Check data provider - does it return `{ data: [], total: 0 }`?

```tsx
// Add logging
const { data, isLoading, error } = useList({ resource: "products" });

console.log("Loading:", isLoading);
console.log("Error:", error);
console.log("Data:", data);
```

---

### Authentication redirects

**Problem:** Stuck in redirect loop

**Solution:**
```tsx
// Check authProvider.check()
check: async () => {
  const token = localStorage.getItem("token");

  // Ensure it returns correct shape
  if (token) {
    return { authenticated: true };  // ✅ Correct
  }

  return {
    authenticated: false,
    redirectTo: "/login",
  };
}
```

---

### Form not submitting

**Debug:**
```tsx
const { onFinish, formLoading } = useForm();

const handleSubmit = async (values) => {
  console.log("Submitting:", values);  // ← Check values

  try {
    const result = await onFinish(values);
    console.log("Result:", result);  // ← Check result
  } catch (error) {
    console.error("Error:", error);  // ← Check error
  }
};
```

---

## Network Debugging

### Inspect Requests

Open Network tab in DevTools:

1. Filter by XHR/Fetch
2. Check request URL
3. Check request payload
4. Check response status
5. Check response data

---

### Check Data Provider

```tsx
// Add logging to data provider
export const dataProvider = {
  getList: async (params) => {
    console.log("getList params:", params);

    const response = await fetch(/* ... */);
    const data = await response.json();

    console.log("getList response:", data);

    return data;
  },
};
```

---

## Performance Debugging

### React Profiler

```tsx
import { Profiler } from "react";

<Profiler id="ProductList" onRender={(id, phase, actualDuration) => {
  console.log(`${id} (${phase}): ${actualDuration}ms`);
}}>
  <ProductList />
</Profiler>
```

### Find Slow Queries

Check TanStack Query DevTools:
- Look for slow queries (> 1s)
- Check for redundant fetches
- Verify caching is working

---

## Error Messages

### "Cannot read property 'X' of undefined"

**Cause:** Trying to access property before data loads

**Fix:**
```tsx
// ❌ Bad
<div>{data.items.map(/* ... */)}</div>

// ✅ Good
<div>{data?.items?.map(/* ... */)}</div>

// ✅ Better
if (!data) return <div>Loading...</div>;
return <div>{data.items.map(/* ... */)}</div>;
```

---

## Getting Help

**Still stuck?**
- [Discord](https://discord.gg/refine)
- [GitHub Discussions](https://github.com/refinedev/refine/discussions)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/refine)

**Before asking:**
- Check console for errors
- Check network tab
- Try in a fresh example
- Provide minimal reproduction

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
