# Hooks Guide

**📅 Documented:** November 18, 2025
**🔬 Tech Stack:** [View Research](TECH_STACK_RESEARCH.md)
**👥 Audience:** Active developers
**⏱️ Estimated Reading:** 40-50 minutes

---

## Introduction

Refine provides **30+ React hooks** for all CRUD operations, forms, tables, auth, navigation, and more.

**🧠 Mental Model:** Hooks are your toolkit - each solves a specific problem.

---

## Data Fetching Hooks

### useList()

Fetch paginated, filtered, sorted list of records.

```tsx
import { useList } from "@refinedev/core";

function ProductList() {
  const { data, isLoading, error } = useList({
    resource: "products",
    pagination: {
      current: 1,
      pageSize: 10,
    },
    filters: [
      {
        field: "category",
        operator: "eq",
        value: "phones",
      },
    ],
    sorters: [
      {
        field: "price",
        order: "desc",
      },
    ],
    meta: {
      headers: { "X-Custom": "value" },
    },
  });

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <table>
      {data?.data.map((product) => (
        <tr key={product.id}>
          <td>{product.name}</td>
          <td>${product.price}</td>
        </tr>
      ))}
    </table>
  );
}
```

**Return Type:**
```typescript
{
  data: { data: T[], total: number } | undefined;
  isLoading: boolean;
  isFetching: boolean;
  isError: boolean;
  error: HttpError | null;
  refetch: () => void;
}
```

---

### useOne()

Fetch a single record by ID.

```tsx
const { data, isLoading } = useOne({
  resource: "products",
  id: 123,
  meta: {
    fields: ["id", "name", "price", "description"],
  },
});

// data.data = { id: 123, name: "iPhone", price: 999, ... }
```

**Use Cases:**
- Show/detail pages
- Fetching related data
- Getting current user

---

### useMany()

Fetch multiple specific records by IDs.

```tsx
const { data } = useMany({
  resource: "categories",
  ids: [1, 2, 3, 5, 8],
});

// data.data = [
//   { id: 1, name: "Phones" },
//   { id: 2, name: "Laptops" },
//   ...
// ]
```

**Common Pattern:**
```tsx
// Get products
const { data: products } = useList({ resource: "products" });

// Extract category IDs
const categoryIds = products?.data.map(p => p.categoryId) ?? [];

// Fetch categories in one request
const { data: categories } = useMany({
  resource: "categories",
  ids: categoryIds,
  queryOptions: {
    enabled: categoryIds.length > 0,  // Only fetch if we have IDs
  },
});
```

---

### useInfiniteList()

Infinite scrolling pagination.

```tsx
const {
  data,
  fetchNextPage,
  hasNextPage,
  isFetchingNextPage,
} = useInfiniteList({
  resource: "products",
  pagination: {
    pageSize: 20,
  },
});

// data.pages = [
//   { data: [...20 items], total: 100 },
//   { data: [...20 items], total: 100 },
//   { data: [...20 items], total: 100 },
// ]

return (
  <div>
    {data?.pages.map((page, i) => (
      <div key={i}>
        {page.data.map((product) => (
          <ProductCard key={product.id} {...product} />
        ))}
      </div>
    ))}
    {hasNextPage && (
      <button onClick={() => fetchNextPage()}>
        {isFetchingNextPage ? "Loading..." : "Load More"}
      </button>
    )}
  </div>
);
```

---

## Mutation Hooks

### useCreate()

Create a new record.

```tsx
const { mutate, isLoading } = useCreate();

const handleSubmit = (values) => {
  mutate({
    resource: "products",
    values: {
      name: values.name,
      price: values.price,
    },
    successNotification: {
      message: "Product created successfully!",
      description: "Success",
      type: "success",
    },
    errorNotification: {
      message: "Failed to create product",
      description: "Error",
      type: "error",
    },
  });
};

return (
  <form onSubmit={handleSubmit}>
    <input name="name" />
    <input name="price" type="number" />
    <button type="submit" disabled={isLoading}>
      {isLoading ? "Creating..." : "Create"}
    </button>
  </form>
);
```

**Options:**
```typescript
{
  mutationMode?: "pessimistic" | "optimistic" | "undoable";
  onSuccess?: (data, variables, context) => void;
  onError?: (error, variables, context) => void;
  invalidates?: string[];  // Queries to invalidate
}
```

---

### useUpdate()

Update an existing record.

```tsx
const { mutate } = useUpdate();

mutate({
  resource: "products",
  id: 123,
  values: {
    price: 899,  // Update price
  },
  mutationMode: "optimistic",  // Update UI immediately
});
```

---

### useDelete()

Delete a record.

```tsx
const { mutate } = useDelete();

const handleDelete = (id) => {
  if (confirm("Are you sure?")) {
    mutate({
      resource: "products",
      id,
      mutationMode: "undoable",  // Show undo notification
    });
  }
};

return <button onClick={() => handleDelete(123)}>Delete</button>;
```

---

### useCreateMany(), useUpdateMany(), useDeleteMany()

Batch operations.

```tsx
const { mutate } = useCreateMany();

mutate({
  resource: "products",
  values: [
    { name: "Product 1", price: 100 },
    { name: "Product 2", price: 200 },
    { name: "Product 3", price: 300 },
  ],
});
```

---

## Form Hooks

### useForm()

Complete form management with validation, submission, and auto-save.

```tsx
import { useForm } from "@refinedev/core";

function ProductEdit() {
  const {
    formProps,
    saveButtonProps,
    queryResult,
    formLoading,
    onFinish,
  } = useForm({
    resource: "products",
    action: "edit",  // "create" | "edit" | "clone"
    id: 123,
    redirect: "list",  // Where to go after save
    warnWhenUnsavedChanges: true,
    autoSave: {
      enabled: true,
      interval: 2000,  // Auto-save every 2 seconds
    },
  });

  return (
    <form {...formProps}>
      <input name="name" defaultValue={queryResult?.data?.data.name} />
      <input name="price" type="number" defaultValue={queryResult?.data?.data.price} />
      <button {...saveButtonProps}>
        {formLoading ? "Saving..." : "Save"}
      </button>
    </form>
  );
}
```

**With React Hook Form:**
```tsx
import { useForm } from "@refinedev/react-hook-form";

function ProductForm() {
  const {
    refineCore: { formLoading, onFinish },
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  return (
    <form onSubmit={handleSubmit(onFinish)}>
      <input
        {...register("name", { required: "Name is required" })}
      />
      {errors.name && <span>{errors.name.message}</span>}

      <input
        {...register("price", { min: { value: 0, message: "Price must be positive" } })}
        type="number"
      />
      {errors.price && <span>{errors.price.message}</span>}

      <button type="submit" disabled={formLoading}>
        Save
      </button>
    </form>
  );
}
```

---

### useStepsForm()

Multi-step form wizard.

```tsx
const {
  current,
  gotoStep,
  stepsProps,
  formProps,
  saveButtonProps,
} = useStepsForm({
  resource: "products",
});

return (
  <div>
    <Steps {...stepsProps}>
      <Step title="Basic Info" />
      <Step title="Pricing" />
      <Step title="Images" />
    </Steps>

    <form {...formProps}>
      {current === 0 && (
        <div>
          <input name="name" />
          <input name="description" />
        </div>
      )}
      {current === 1 && (
        <div>
          <input name="price" type="number" />
          <input name="discount" type="number" />
        </div>
      )}
      {current === 2 && (
        <div>
          <input name="images" type="file" multiple />
        </div>
      )}

      {current > 0 && <button onClick={() => gotoStep(current - 1)}>Previous</button>}
      {current < 2 && <button onClick={() => gotoStep(current + 1)}>Next</button>}
      {current === 2 && <button {...saveButtonProps}>Submit</button>}
    </form>
  </div>
);
```

---

## Table Hooks

### useTable()

Complete table management with sorting, filtering, pagination.

```tsx
const {
  tableQueryResult,
  current,
  setCurrent,
  pageSize,
  setPageSize,
  sorters,
  setSorters,
  filters,
  setFilters,
} = useTable({
  resource: "products",
  initialSorter: [
    {
      field: "createdAt",
      order: "desc",
    },
  ],
  initialFilter: [
    {
      field: "status",
      operator: "eq",
      value: "active",
    },
  ],
  syncWithLocation: true,  // Sync with URL query params
});

const data = tableQueryResult.data?.data ?? [];
const total = tableQueryResult.data?.total ?? 0;

return (
  <div>
    <input
      placeholder="Search..."
      onChange={(e) => {
        setFilters([
          {
            field: "name",
            operator: "contains",
            value: e.target.value,
          },
        ]);
      }}
    />

    <table>
      <thead>
        <tr>
          <th
            onClick={() => {
              setSorters([
                {
                  field: "name",
                  order: sorters[0]?.order === "asc" ? "desc" : "asc",
                },
              ]);
            }}
          >
            Name {sorters[0]?.field === "name" && (sorters[0].order === "asc" ? "↑" : "↓")}
          </th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>
        {data.map((product) => (
          <tr key={product.id}>
            <td>{product.name}</td>
            <td>${product.price}</td>
          </tr>
        ))}
      </tbody>
    </table>

    <div>
      Page {current} of {Math.ceil(total / pageSize)}
      <button onClick={() => setCurrent(current - 1)} disabled={current === 1}>
        Previous
      </button>
      <button onClick={() => setCurrent(current + 1)} disabled={current * pageSize >= total}>
        Next
      </button>
    </div>
  </div>
);
```

---

## Auth Hooks

### useLogin()

```tsx
const { mutate: login, isLoading } = useLogin();

const handleSubmit = (values) => {
  login({
    email: values.email,
    password: values.password,
  });
};
```

### useLogout()

```tsx
const { mutate: logout } = useLogout();

return <button onClick={() => logout()}>Logout</button>;
```

### usePermissions()

```tsx
const { data: permissions, isLoading } = usePermissions();

if (permissions?.includes("admin")) {
  return <AdminPanel />;
}
```

### useGetIdentity()

```tsx
const { data: user } = useGetIdentity();

return <div>Welcome, {user?.name}!</div>;
```

### useIsAuthenticated()

```tsx
const { data: authData, isLoading } = useIsAuthenticated();

if (isLoading) return <div>Checking auth...</div>;
if (!authData?.authenticated) return <Login />;

return <Dashboard />;
```

---

## Navigation Hooks

### useGo()

Programmatic navigation.

```tsx
const go = useGo();

// Navigate to resource
go({
  to: {
    resource: "products",
    action: "list",
  },
});

// Navigate to specific ID
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
    category: "phones",
    sortBy: "price",
  },
});

// External URL
go({
  to: "https://example.com",
  type: "external",
});
```

### useBack()

```tsx
const back = useBack();

return <button onClick={() => back()}>Go Back</button>;
```

### useParsed()

Parse current route information.

```tsx
const { resource, action, id, pathname } = useParsed();

console.log(resource); // "products"
console.log(action);   // "edit"
console.log(id);       // "123"
console.log(pathname); // "/products/edit/123"
```

---

## Utility Hooks

### useSelect()

Populate select/dropdown with data from a resource.

```tsx
const { selectProps, queryResult } = useSelect({
  resource: "categories",
  optionLabel: "name",
  optionValue: "id",
  defaultValue: selectedCategoryId,
  filters: [
    {
      field: "status",
      operator: "eq",
      value: "active",
    },
  ],
});

return (
  <select {...selectProps}>
    {/* Options populated automatically */}
  </select>
);
```

With search:
```tsx
const { selectProps, queryResult, onSearch } = useSelect({
  resource: "products",
  debounce: 500,  // Debounce search
});

return (
  <input
    onChange={(e) => onSearch(e.target.value)}
    placeholder="Search products..."
  />
  <select {...selectProps} />
);
```

---

### useNotification()

```tsx
const { open, close } = useNotification();

open({
  type: "success",
  message: "Operation successful!",
  description: "The product was created.",
});

// Auto-close after 5 seconds (default)
// Or close manually
close("notification-key");
```

---

### useTranslate()

```tsx
const translate = useTranslate();

return (
  <div>
    <h1>{translate("products.titles.list")}</h1>
    <button>{translate("buttons.create")}</button>
    <p>{translate("messages.itemCount", { count: 5 })}</p>
  </div>
);
```

---

### useInvalidate()

Manually invalidate queries to trigger refetch.

```tsx
const invalidate = useInvalidate();

// Invalidate all product list queries
invalidate({
  resource: "products",
  invalidates: ["list"],
});

// Invalidate specific product
invalidate({
  resource: "products",
  invalidates: ["detail"],
  id: 123,
});

// Invalidate all queries
invalidate({
  resource: "products",
  invalidates: ["all"],
});
```

---

## Resource & Router Hooks

### useResource()

Get current resource context.

```tsx
const { resource, id, action } = useResource();

console.log(resource?.name); // "products"
console.log(id);             // "123"
console.log(action);         // "edit"
```

### useNavigation()

Navigation helpers for resources.

```tsx
const {
  list,
  create,
  edit,
  show,
  clone,
} = useNavigation();

// Navigate to list
list("products");

// Navigate to create
create("products");

// Navigate to edit
edit("products", 123);

// Navigate to show
show("products", 123);
```

---

## UI State Hooks

### useModal()

```tsx
const {
  show,
  close,
  visible,
  modalProps,
} = useModal();

return (
  <>
    <button onClick={show}>Open Modal</button>
    <Modal {...modalProps}>
      <p>Modal content</p>
      <button onClick={close}>Close</button>
    </Modal>
  </>
);
```

### useDrawer()

Same API as useModal but for drawers/sidebars.

---

## Hook Composition Patterns

### Pattern 1: Master-Detail

```tsx
function ProductListWithDetail() {
  const [selectedId, setSelectedId] = useState<number | null>(null);

  // List
  const { data: products } = useList({ resource: "products" });

  // Detail (only fetch when selected)
  const { data: selectedProduct } = useOne({
    resource: "products",
    id: selectedId!,
    queryOptions: {
      enabled: selectedId !== null,  // Only fetch if ID set
    },
  });

  return (
    <div>
      <ul>
        {products?.data.map((product) => (
          <li key={product.id} onClick={() => setSelectedId(product.id)}>
            {product.name}
          </li>
        ))}
      </ul>

      {selectedProduct && (
        <div>
          <h2>{selectedProduct.data.name}</h2>
          <p>{selectedProduct.data.description}</p>
        </div>
      )}
    </div>
  );
}
```

### Pattern 2: Dependent Data

```tsx
function ProductFormWithCategories() {
  const { formProps, queryResult } = useForm();

  // Get category IDs from product
  const categoryIds = queryResult?.data?.data.categoryIds ?? [];

  // Fetch categories only when we have IDs
  const { data: categories } = useMany({
    resource: "categories",
    ids: categoryIds,
    queryOptions: {
      enabled: categoryIds.length > 0,
    },
  });

  return (
    <form {...formProps}>
      <div>Categories: {categories?.data.map(c => c.name).join(", ")}</div>
    </form>
  );
}
```

---

## Summary

**Essential Hooks:**
- `useList()` - Fetch lists
- `useOne()` - Fetch single
- `useCreate()` - Create
- `useUpdate()` - Update
- `useDelete()` - Delete
- `useForm()` - Forms
- `useTable()` - Tables

**Auth & Navigation:**
- `useLogin()`, `useLogout()`, `usePermissions()`
- `useGo()`, `useBack()`, `useParsed()`

**Utilities:**
- `useSelect()` - Dropdowns
- `useNotification()` - Messages
- `useTranslate()` - i18n

[Explore all hooks in the code →](../../packages/core/src/hooks/)

---

## Next Steps

**Understand providers:**
→ [Providers Guide](PROVIDERS_GUIDE.md)

**See patterns:**
→ [Patterns & Conventions](PATTERNS_AND_CONVENTIONS.md)

**Build features:**
→ [How-To Guide](HOW_TO_GUIDE.md)

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
