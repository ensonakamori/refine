# Routing Guide

**📅 Documented:** November 18, 2025
**🔬 Tech Stack:** [View Research](TECH_STACK_RESEARCH.md)
**👥 Audience:** Developers choosing a router
**⏱️ Estimated Reading:** 25-30 minutes

---

## Overview

Refine is **router-agnostic** - works with React Router, Next.js, Remix, or custom routers.

**🧠 Mental Model:** Refine doesn't impose routing - you choose your router, Refine adapts.

---

## Router Integrations

### React Router

✅ **CURRENT (Nov 2025):** React Router is industry standard for client-side routing.

```bash
npm install @refinedev/react-router react-router-dom
```

```tsx
import { Refine } from "@refinedev/core";
import routerProvider from "@refinedev/react-router";
import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <Refine
        routerProvider={routerProvider}
        resources={[
          {
            name: "products",
            list: "/products",
            create: "/products/create",
            edit: "/products/edit/:id",
            show: "/products/show/:id",
          },
        ]}
      >
        <Routes>
          <Route
            element={
              <Layout>
                <Outlet />
              </Layout>
            }
          >
            <Route index element={<Dashboard />} />
            <Route path="/products">
              <Route index element={<ProductList />} />
              <Route path="create" element={<ProductCreate />} />
              <Route path="edit/:id" element={<ProductEdit />} />
              <Route path="show/:id" element={<ProductShow />} />
            </Route>
          </Route>
        </Routes>
      </Refine>
    </BrowserRouter>
  );
}
```

---

### Next.js

✅ **CURRENT (Nov 2025):** Next.js App Router is the 2025 standard.

```bash
npm install @refinedev/nextjs-router
```

**App Router (Next.js 13+):**

```tsx
// app/layout.tsx
import { Refine } from "@refinedev/core";
import routerProvider from "@refinedev/nextjs-router/app";

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <Refine
          routerProvider={routerProvider}
          resources={[
            {
              name: "products",
              list: "/products",
              create: "/products/create",
              edit: "/products/edit/:id",
              show: "/products/show/:id",
            },
          ]}
        >
          {children}
        </Refine>
      </body>
    </html>
  );
}

// app/products/page.tsx
export default function ProductsPage() {
  return <ProductList />;
}

// app/products/create/page.tsx
export default function ProductsCreatePage() {
  return <ProductCreate />;
}

// app/products/edit/[id]/page.tsx
export default function ProductsEditPage() {
  return <ProductEdit />;
}
```

**Pages Router (Legacy):**

```tsx
// pages/_app.tsx
import { Refine } from "@refinedev/core";
import routerProvider from "@refinedev/nextjs-router/pages";

function MyApp({ Component, pageProps }) {
  return (
    <Refine
      routerProvider={routerProvider}
      resources={[...]}
    >
      <Component {...pageProps} />
    </Refine>
  );
}
```

⚠️ **OUTDATED PATTERN (Nov 2025):** Pages Router is legacy. Use App Router for new projects.

---

### Remix

```bash
npm install @refinedev/remix-router
```

```tsx
// app/root.tsx
import { Refine } from "@refinedev/core";
import routerProvider from "@refinedev/remix-router";
import { Outlet } from "@remix-run/react";

export default function App() {
  return (
    <html>
      <body>
        <Refine
          routerProvider={routerProvider}
          resources={[...]}
        >
          <Outlet />
        </Refine>
      </body>
    </html>
  );
}

// app/routes/products._index.tsx
export default function ProductsIndex() {
  return <ProductList />;
}

// app/routes/products.create.tsx
export default function ProductsCreate() {
  return <ProductCreate />;
}
```

---

## Resource Routes

### Automatic Route Generation

When you define resources, Refine understands the routes:

```tsx
resources={[
  {
    name: "products",
    list: "/products",
    create: "/products/create",
    edit: "/products/edit/:id",
    show: "/products/show/:id",
  },
]}

// Now hooks know which route to use:
const go = useGo();

go({
  to: {
    resource: "products",
    action: "list",
  },
});
// Navigates to /products

go({
  to: {
    resource: "products",
    action: "edit",
    id: 123,
  },
});
// Navigates to /products/edit/123
```

### Custom Routes

```tsx
resources={[
  {
    name: "products",
    list: "/my-products",              // Custom path
    create: "/my-products/new",         // Custom path
    edit: "/my-products/:id/modify",    // Custom path
    show: "/my-products/:id",           // Custom path
  },
]}
```

---

## Navigation Hooks

### useGo()

Programmatic navigation:

```tsx
const go = useGo();

// Navigate to resource
go({
  to: {
    resource: "products",
    action: "list",
  },
});

// Navigate with ID
go({
  to: {
    resource: "products",
    action: "show",
    id: 123,
  },
});

// Navigate with query params
go({
  to: {
    resource: "products",
    action: "list",
  },
  query: {
    page: 2,
    filters: { category: "phones" },
  },
});

// Navigate to absolute path
go({
  to: "/custom/path",
});

// Navigate to external URL
go({
  to: "https://example.com",
  type: "external",
});

// Replace instead of push
go({
  to: "/products",
  type: "replace",
});
```

### useBack()

Go back in history:

```tsx
const back = useBack();

return <button onClick={() => back()}>Go Back</button>;
```

### useParsed()

Parse current route:

```tsx
const {
  resource,     // Current resource
  action,       // Current action (list, create, edit, show)
  id,           // Current ID (for edit/show)
  pathname,     // Current pathname
  params,       // Route params
  ...} = useParsed();

console.log(resource?.name);  // "products"
console.log(action);          // "edit"
console.log(id);              // "123"
```

---

## Sync with Location

Keep filters/sorters/pagination in URL:

```tsx
const { tableQueryResult, filters, setFilters, /* ... */ } = useTable({
  resource: "products",
  syncWithLocation: true,  // ← Sync with URL
});

// URL becomes:
// /products?current=1&pageSize=10&filters[0][field]=category&filters[0][operator]=eq&filters[0][value]=phones
```

**Benefits:**
- ✅ Shareable URLs
- ✅ Browser back/forward works
- ✅ Refresh preserves state

---

## Route Parameters

### Dynamic Parameters

```tsx
// Route: /products/edit/:id
resources={[
  {
    name: "products",
    edit: "/products/edit/:id",
  },
]}

// In component
function ProductEdit() {
  const { id } = useParsed();  // Get ID from URL

  const { data } = useOne({
    resource: "products",
    id,  // Use ID from URL
  });
}
```

### Multiple Parameters

```tsx
// Route: /users/:userId/posts/:postId/edit
resources={[
  {
    name: "posts",
    edit: "/users/:userId/posts/:postId/edit",
  },
]}

// In component
function PostEdit() {
  const { params } = useParsed();

  console.log(params?.userId);  // From URL
  console.log(params?.postId);  // From URL
}
```

---

## Meta Router Data

Pass custom data through router:

```tsx
// Navigate with meta
go({
  to: {
    resource: "products",
    action: "show",
    id: 123,
  },
  meta: {
    tab: "reviews",  // Custom meta
  },
});

// Access in component
const { params } = useParsed();
console.log(params?.meta?.tab);  // "reviews"
```

---

## Authentication Routes

### Protected Routes

```tsx
import { Authenticated } from "@refinedev/core";

<Routes>
  <Route
    element={
      <Authenticated
        key="authenticated"
        fallback={<Navigate to="/login" />}
      >
        <Layout>
          <Outlet />
        </Layout>
      </Authenticated>
    }
  >
    <Route path="/products" element={<ProductList />} />
    {/* Protected routes */}
  </Route>

  <Route path="/login" element={<Login />} />
</Routes>
```

### Redirect After Login

```tsx
const { mutate: login } = useLogin();

login({
  email,
  password,
  redirectTo: "/dashboard",  // Where to go after login
});
```

---

## Choosing a Router

| Router | Best For | Pros | Cons |
|--------|----------|------|------|
| **React Router** | SPAs, Client-side apps | Simple, flexible, widely used | No SSR out of box |
| **Next.js App Router** | Full-stack apps, SEO important | SSR, RSC, SEO, file-based | More complex |
| **Next.js Pages** | Legacy Next.js apps | Simpler than App Router | ⚠️ Legacy (use App Router) |
| **Remix** | Full-stack apps, progressive enhancement | Great DX, web standards | Smaller ecosystem |

✅ **CURRENT (Nov 2025) Recommendations:**
- **SPA:** React Router
- **SEO/Full-stack:** Next.js App Router
- **Progressive Enhancement:** Remix

---

## Best Practices

### 1. Use Resource-Based Navigation

```tsx
// ✅ Good - uses resource system
go({ to: { resource: "products", action: "list" } });

// ❌ Avoid - hardcoded path
go({ to: "/products" });
```

### 2. Sync Important State with URL

```tsx
// ✅ Good - shareable URLs
useTable({ syncWithLocation: true });

// ❌ Avoid - state lost on refresh
const [page, setPage] = useState(1);
```

### 3. Use Type-Safe Navigation

```tsx
// TypeScript ensures correct resource/action
go({
  to: {
    resource: "products",  // ✅ Autocompleted
    action: "list",        // ✅ Type-checked
  },
});
```

---

## Summary

**Router Choice:**
- React Router - SPAs
- Next.js App Router - Full-stack (2025 standard)
- Remix - Web standards

**Navigation:**
- `useGo()` - Navigate
- `useBack()` - Go back
- `useParsed()` - Current route info

**Features:**
- Automatic route generation from resources
- Sync state with URL
- Protected routes with `<Authenticated>`

---

## Next Steps

**Learn UI integrations:**
→ [UI Integrations Guide](UI_INTEGRATIONS_GUIDE.md)

**See patterns:**
→ [Patterns & Conventions](PATTERNS_AND_CONVENTIONS.md)

**Build something:**
→ [How-To Guide](HOW_TO_GUIDE.md)

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
