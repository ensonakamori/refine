# How-To Guide

**📅 Documented:** November 18, 2025
**👥 Audience:** All developers
**⏱️ Estimated Reading:** 30-40 minutes

---

## How to Create a Resource

1. Define in `<Refine />`:

```tsx
<Refine
  resources={[
    {
      name: "products",
      list: "/products",
      create: "/products/create",
      edit: "/products/edit/:id",
      show: "/products/show/:id",
    },
  ]}
/>
```

2. Create pages:
   - `pages/products/list.tsx`
   - `pages/products/create.tsx`
   - `pages/products/edit.tsx`
   - `pages/products/show.tsx`

3. Add routes in your router

---

## How to Add Authentication

1. Create auth provider:

```tsx
const authProvider = {
  login: async ({ email, password }) => {
    const { data } = await api.post("/login", { email, password });
    localStorage.setItem("token", data.token);
    return { success: true };
  },
  logout: async () => {
    localStorage.removeItem("token");
    return { success: true };
  },
  check: async () => {
    return { authenticated: !!localStorage.getItem("token") };
  },
  getIdentity: async () => {
    const token = localStorage.getItem("token");
    if (!token) return null;
    const { data } = await api.get("/me");
    return data;
  },
};
```

2. Add to Refine:

```tsx
<Refine authProvider={authProvider} />
```

3. Protect routes:

```tsx
<Authenticated fallback={<Navigate to="/login" />}>
  <Dashboard />
</Authenticated>
```

---

## How to Add Filtering

```tsx
const { tableProps } = useTable({
  initialFilter: [
    {
      field: "category",
      operator: "eq",
      value: "phones",
    },
  ],
});
```

---

## How to Add Sorting

```tsx
const { tableProps } = useTable({
  initialSorter: [
    {
      field: "createdAt",
      order: "desc",
    },
  ],
});
```

---

## How to Add Real-Time Updates

1. Setup live provider:

```tsx
import { liveProvider } from "@refinedev/supabase";

<Refine
  liveProvider={liveProvider(supabaseClient)}
  options={{ liveMode: "auto" }}
/>
```

2. Enable on hooks:

```tsx
useList({
  resource: "products",
  liveMode: "auto",  // Subscribes to changes
});
```

---

## How to Add Internationalization

1. Create i18n provider:

```tsx
const i18nProvider = {
  translate: (key) => translations[locale][key],
  changeLocale: (newLocale) => { locale = newLocale; },
  getLocale: () => locale,
};
```

2. Add to Refine:

```tsx
<Refine i18nProvider={i18nProvider} />
```

3. Use translations:

```tsx
const translate = useTranslate();
<h1>{translate("products.title")}</h1>
```

---

## How to Add Access Control

1. Create access control provider:

```tsx
const accessControlProvider = {
  can: async ({ resource, action }) => {
    if (resource === "products" && action === "delete") {
      return { can: user.role === "admin" };
    }
    return { can: true };
  },
};
```

2. Use in components:

```tsx
<CanAccess resource="products" action="delete">
  <DeleteButton />
</CanAccess>
```

---

## How to Customize Forms

```tsx
const { formProps, saveButtonProps } = useForm({
  refineCoreProps: {
    onMutationSuccess: () => {
      message.success("Saved!");
      navigate("/products");
    },
    onMutationError: (error) => {
      message.error(error.message);
    },
  },
});
```

---

## How to Add Custom Endpoints

```tsx
const { data } = useCustom({
  url: "/api/custom-endpoint",
  method: "post",
  config: {
    payload: { customData: "value" },
  },
});
```

---

## Summary

This guide covers common tasks. See specific guides for details:
- [Authentication](CORE_CONCEPTS.md#authentication)
- [Filtering](CORE_CONCEPTS.md#filtering--sorting)
- [Real-Time](PROVIDERS_GUIDE.md#live-provider)

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
