# Core Concepts

**📅 Documented:** November 18, 2025
**🔬 Tech Stack:** [View Research](TECH_STACK_RESEARCH.md)
**👥 Audience:** Developers building with Refine
**⏱️ Estimated Reading:** 30-40 minutes

---

## Introduction

Refine is built on **fundamental concepts** that once understood, make everything else click.

**The Big Three:**
1. **Resources** - What you manage
2. **Providers** - How you connect to services
3. **Hooks** - How you interact with everything

---

## Resources

### What is a Resource?

A **resource** represents an entity in your application (users, products, orders, etc.).

**🧠 Mental Model:** Resources are like database tables or REST endpoints.

### Resource Definition

```tsx
<Refine
  resources={[
    {
      name: "products",              // Identifier (matches API endpoint)
      list: "/products",             // List route
      create: "/products/create",    // Create route
      edit: "/products/edit/:id",    // Edit route
      show: "/products/show/:id",    // Show route
      meta: {
        label: "Products",           // Display name
        icon: <ProductIcon />,       // Menu icon
        canDelete: true,             // Can delete?
        parent: "catalog",           // Parent menu item
      }
    }
  ]}
/>
```

### Resource Actions

| Action | HTTP | Hook | Route |
|--------|------|------|-------|
| **list** | GET /products | `useList` | /products |
| **create** | POST /products | `useCreate` | /products/create |
| **edit** | PATCH /products/:id | `useUpdate` | /products/edit/:id |
| **show** | GET /products/:id | `useOne` | /products/show/:id |
| **delete** | DELETE /products/:id | `useDelete` | - |

### Resources Are Central

Once you define a resource, Refine automatically:
- ✅ Generates menu items
- ✅ Generates routes
- ✅ Generates breadcrumbs
- ✅ Provides hooks that know the resource context
- ✅ Handles navigation

**Example:**

```tsx
// You define:
{ name: "products", list: "/products" }

// You get for free:
- Menu item "Products"
- Route /products
- Breadcrumb "Home / Products"
- useList({ resource: "products" }) works
- Navigation helpers understand "products"
```

### Nested Resources

```tsx
resources={[
  {
    name: "users",
    list: "/users"
  },
  {
    name: "users/posts",          // Nested resource
    list: "/users/:userId/posts", // Dynamic route
    meta: {
      parent: "users"             // Groups in menu
    }
  }
]}
```

---

## Providers

Providers are **adapters** between Refine and your infrastructure.

### Provider Types

**1. Data Provider**
```typescript
interface DataProvider {
  getList: (params) => Promise<{ data: T[], total: number }>;
  getOne: (params) => Promise<{ data: T }>;
  create: (params) => Promise<{ data: T }>;
  update: (params) => Promise<{ data: T }>;
  deleteOne: (params) => Promise<{ data: T }>;
  getMany?: (params) => Promise<{ data: T[] }>;
  createMany?: (params) => Promise<{ data: T[] }>;
  updateMany?: (params) => Promise<{ data: T[] }>;
  deleteMany?: (params) => Promise<{ data: T[] }>;
  custom?: (params) => Promise<{ data: any }>;
}
```

**2. Auth Provider**
```typescript
interface AuthProvider {
  login: (params) => Promise<{ success: boolean }>;
  logout: (params) => Promise<{ success: boolean }>;
  check: (params) => Promise<{ authenticated: boolean }>;
  onError: (error) => Promise<{ error: any }>;
  getPermissions: () => Promise<any>;
  getIdentity?: () => Promise<{ id: any; name: string }>;
}
```

**3. Router Provider**
```typescript
interface RouterProvider {
  go: (config) => void;
  back: () => void;
  parse: () => ParseResponse;
}
```

**4. I18n Provider**
```typescript
interface I18nProvider {
  translate: (key: string, options?: any) => string;
  changeLocale: (locale: string) => Promise<any>;
  getLocale: () => string;
}
```

**5. Notification Provider**
```typescript
interface NotificationProvider {
  open: (params: {
    message: string;
    description?: string;
    type: "success" | "error" | "info";
  }) => void;
  close?: (key: any) => void;
}
```

**6. Access Control Provider**
```typescript
interface AccessControlProvider {
  can: (params: {
    resource: string;
    action: string;
    params?: any;
  }) => Promise<{ can: boolean; reason?: string }>;
}
```

**7. Audit Log Provider**
```typescript
interface AuditLogProvider {
  create: (params: {
    resource: string;
    action: string;
    data: any;
    meta?: any;
  }) => Promise<any>;
  get: (params) => Promise<any>;
  update: (params) => Promise<any>;
}
```

**8. Live Provider**
```typescript
interface LiveProvider {
  subscribe: (params: {
    channel: string;
    types: ("*" | "created" | "updated" | "deleted")[];
    callback: (event) => void;
  }) => any;
  unsubscribe: (subscription: any) => void;
  publish?: (event) => void;
}
```

### Why Providers?

**Swappable:**
```tsx
// Development
<Refine dataProvider={fakeRestProvider} />

// Production
<Refine dataProvider={realRestProvider} />
```

**Multiple Providers:**
```tsx
<Refine
  dataProvider={{
    default: restProvider,
    graphql: graphqlProvider,
  }}
/>

// Use different providers
useList({ dataProviderName: "default" });
useList({ dataProviderName: "graphql" });
```

**Easy Testing:**
```tsx
const mockProvider = {
  getList: jest.fn(() => Promise.resolve({ data: [], total: 0 }))
};
```

---

## Hooks

Refine provides **30+ hooks** for all operations.

### Hook Categories

**Data Hooks:**
- `useList()` - Fetch paginated list
- `useOne()` - Fetch single record
- `useMany()` - Fetch multiple specific records
- `useInfiniteList()` - Infinite scroll
- `useCreate()` - Create record
- `useCreateMany()` - Create multiple
- `useUpdate()` - Update record
- `useUpdateMany()` - Update multiple
- `useDelete()` - Delete record
- `useDeleteMany()` - Delete multiple
- `useCustom()` - Custom endpoint

**Form Hooks:**
- `useForm()` - Complete form management
- `useStepsForm()` - Multi-step forms
- `useDrawerForm()` - Form in drawer
- `useModalForm()` - Form in modal

**Table Hooks:**
- `useTable()` - Complete table management
- `useDataGrid()` - DataGrid integration (MUI)

**Auth Hooks:**
- `useLogin()` - Login mutation
- `useLogout()` - Logout mutation
- `useRegister()` - Registration
- `useCheckError()` - Error checker
- `usePermissions()` - Get permissions
- `useGetIdentity()` - Get user info
- `useIsAuthenticated()` - Check auth status

**Navigation Hooks:**
- `useGo()` - Navigate
- `useBack()` - Go back
- `useParsed()` - Parse current route
- `useLink()` - Create links

**Resource Hooks:**
- `useResource()` - Get resource context
- `useResourceParams()` - Get resource params
- `useNavigation()` - Navigation helpers

**UI Hooks:**
- `useModal()` - Modal state
- `useDrawer()` - Drawer state
- `useMenu()` - Menu generation
- `useBreadcrumb()` - Breadcrumb generation

**Utility Hooks:**
- `useSelect()` - Select/dropdown data
- `useNotification()` - Show notifications
- `useTranslate()` - Translations
- `useInvalidate()` - Cache invalidation

[Complete hooks reference →](HOOKS_GUIDE.md)

### Hook Composition

```tsx
function ProductEdit() {
  // Get resource from URL
  const { resource, id } = useResource();

  // Form management
  const {
    formProps,
    saveButtonProps,
    queryResult
  } = useForm();

  // Get category options
  const { selectProps: categorySelectProps } = useSelect({
    resource: "categories"
  });

  // Check permission
  const { data: canEdit } = usePermissions();

  // Get user
  const { data: identity } = useGetIdentity();

  // Navigation
  const go = useGo();

  // Compose them!
  return (
    <form {...formProps}>
      <input name="name" />
      <select {...categorySelectProps} />
      <button {...saveButtonProps}>Save</button>
    </form>
  );
}
```

---

## Meta Data

**Meta** allows passing custom data through hooks to providers.

### Use Cases

**1. Custom Headers:**
```tsx
useList({
  resource: "products",
  meta: {
    headers: {
      "X-Custom-Header": "value"
    }
  }
});

// In data provider
const dataProvider = {
  getList: async ({ resource, meta }) => {
    return axios.get(url, {
      headers: meta?.headers  // ← Use custom headers
    });
  }
};
```

**2. GraphQL Fields:**
```tsx
useList({
  resource: "products",
  meta: {
    fields: ["id", "name", "price", "category { id, name }"]
  }
});

// Provider uses fields to construct GraphQL query
```

**3. Custom Query Params:**
```tsx
useOne({
  resource: "products",
  id: 123,
  meta: {
    include: "reviews,images"
  }
});

// Data provider: GET /products/123?include=reviews,images
```

**4. Operation Metadata:**
```tsx
useCreate({
  resource: "products",
  values: { name: "Phone" },
  meta: {
    operation: "bulk-import",
    source: "csv"
  }
});
```

---

## Mutation Modes

Refine supports three mutation strategies.

### 1. Pessimistic (Default)

**Wait for server before updating UI:**

```tsx
<Refine options={{ mutationMode: "pessimistic" }} />

// Timeline:
T+0ms:   User clicks "Save"
T+0ms:   Show loading spinner
T+300ms: Server responds success
T+300ms: Update UI
T+300ms: Hide loading spinner
```

**Use when:**
- Server validation is critical
- Can't afford rollbacks
- Network is reliable

### 2. Optimistic

**Update UI immediately, rollback if error:**

```tsx
<Refine options={{ mutationMode: "optimistic" }} />

// Timeline:
T+0ms:   User clicks "Save"
T+0ms:   Update UI immediately
T+300ms: Server responds
T+300ms: If error, rollback UI
```

**Use when:**
- Want snappy UX
- Network is reliable
- Errors are rare

### 3. Undoable

**Update UI with undo option:**

```tsx
<Refine options={{ mutationMode: "undoable" }} />

// Timeline:
T+0ms:    User clicks "Save"
T+0ms:    Update UI
T+0ms:    Show "Undo" notification (5 seconds)
T+5000ms: If not undone, send to server
```

**Use when:**
- Want to allow undo
- Batch operations
- Accidental actions possible

### Per-Operation Override

```tsx
// Global: optimistic
<Refine options={{ mutationMode: "optimistic" }} />

// Override for one operation: pessimistic
useDelete({
  mutationMode: "pessimistic"  // ← Override for delete only
});
```

---

## Filtering & Sorting

### Filters

```tsx
useList({
  resource: "products",
  filters: [
    {
      field: "category",
      operator: "eq",
      value: "phones"
    },
    {
      field: "price",
      operator: "gte",
      value: 500
    },
    {
      field: "name",
      operator: "contains",
      value: "iPhone"
    }
  ]
});
```

**Operators:**
- `eq` - Equals
- `ne` - Not equals
- `lt` - Less than
- `lte` - Less than or equal
- `gt` - Greater than
- `gte` - Greater than or equal
- `in` - In array
- `nin` - Not in array
- `contains` - Contains substring
- `ncontains` - Doesn't contain
- `containss` - Contains (case sensitive)
- `ncontainss` - Doesn't contain (case sensitive)
- `null` - Is null
- `nnull` - Is not null
- `between` - Between two values
- `nbetween` - Not between

**Logical Filters:**
```tsx
filters: [
  {
    operator: "or",
    value: [
      { field: "category", operator: "eq", value: "phones" },
      { field: "category", operator: "eq", value: "tablets" }
    ]
  }
]
```

### Sorters

```tsx
useList({
  resource: "products",
  sorters: [
    {
      field: "price",
      order: "desc"  // desc | asc
    },
    {
      field: "name",
      order: "asc"
    }
  ]
});
```

### Pagination

```tsx
useList({
  resource: "products",
  pagination: {
    current: 1,    // Page number
    pageSize: 10,  // Items per page
    mode: "server" // "server" | "client" | "off"
  }
});
```

---

## Internationalization (i18n)

```tsx
// Setup i18n provider
const i18nProvider = {
  translate: (key, options) => {
    return translations[currentLocale][key];
  },
  changeLocale: (locale) => {
    currentLocale = locale;
  },
  getLocale: () => currentLocale
};

<Refine i18nProvider={i18nProvider} />

// Use in components
const translate = useTranslate();

translate("products.titles.list");  // "Product List"
translate("buttons.save");           // "Save"
translate("messages.success", { count: 5 });  // "5 items saved"
```

---

## Access Control

```tsx
// Setup access control
const accessControlProvider = {
  can: async ({ resource, action }) => {
    // Check user permissions
    if (resource === "products" && action === "delete") {
      return { can: user.role === "admin" };
    }
    return { can: true };
  }
};

<Refine accessControlProvider={accessControlProvider} />

// Use in components
const { data } = useCan({
  resource: "products",
  action: "delete"
});

if (data?.can) {
  return <DeleteButton />;
}

// Or with wrapper
<CanAccess resource="products" action="delete">
  <DeleteButton />
</CanAccess>
```

---

## Summary

**The Foundation:**
- **Resources** = What you manage (products, users, etc.)
- **Providers** = How you connect (data, auth, routing, etc.)
- **Hooks** = How you interact (useList, useCreate, etc.)

**Supporting Concepts:**
- **Meta** = Pass custom data
- **Mutation Modes** = How mutations behave
- **Filters/Sorters** = Query your data
- **i18n** = Multi-language support
- **Access Control** = Permissions

**🎯 Master these, master Refine!**

---

## Next Steps

**Deep dive into hooks:**
→ [Hooks Guide](HOOKS_GUIDE.md)

**Understand providers:**
→ [Providers Guide](PROVIDERS_GUIDE.md)

**See it in action:**
→ [Data Flow Guide](DATA_FLOW_GUIDE.md)

**Build something:**
→ [How-To Guide](HOW_TO_GUIDE.md)

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
