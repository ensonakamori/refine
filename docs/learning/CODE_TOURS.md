# Code Tours

**📅 Documented:** November 18, 2025
**👥 Audience:** Learning developers
**⏱️ Estimated Reading:** 30-40 minutes

---

## Tour 1: Simple CRUD App

Follow along to build a complete product management app.

### Step 1: Setup

```tsx
// src/App.tsx
import { Refine } from "@refinedev/core";
import routerProvider from "@refinedev/react-router";
import dataProvider from "@refinedev/simple-rest";

export default function App() {
  return (
    <Refine
      dataProvider={dataProvider("https://api.fake-rest.refine.dev")}
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
      {/* Routes will go here */}
    </Refine>
  );
}
```

### Step 2: List Page

```tsx
// src/pages/products/list.tsx
import { useTable } from "@refinedev/core";

export const ProductList = () => {
  const {
    tableQueryResult: { data, isLoading },
    current,
    setCurrent,
    pageSize,
  } = useTable({
    resource: "products",
    initialPageSize: 10,
  });

  if (isLoading) return <div>Loading...</div>;

  return (
    <div>
      <h1>Products</h1>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Price</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {data?.data.map((product) => (
            <tr key={product.id}>
              <td>{product.id}</td>
              <td>{product.name}</td>
              <td>${product.price}</td>
              <td>
                <a href={`/products/edit/${product.id}`}>Edit</a>
                <a href={`/products/show/${product.id}`}>Show</a>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
      <div>
        <button
          onClick={() => setCurrent(current - 1)}
          disabled={current === 1}
        >
          Previous
        </button>
        <span>Page {current}</span>
        <button onClick={() => setCurrent(current + 1)}>Next</button>
      </div>
    </div>
  );
};
```

### Step 3: Create Page

```tsx
// src/pages/products/create.tsx
import { useForm } from "@refinedev/core";

export const ProductCreate = () => {
  const { onFinish, formLoading } = useForm({
    resource: "products",
    action: "create",
    redirect: "list",
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    onFinish({
      name: formData.get("name"),
      price: parseFloat(formData.get("price")),
    });
  };

  return (
    <div>
      <h1>Create Product</h1>
      <form onSubmit={handleSubmit}>
        <div>
          <label>Name:</label>
          <input name="name" required />
        </div>
        <div>
          <label>Price:</label>
          <input name="price" type="number" step="0.01" required />
        </div>
        <button type="submit" disabled={formLoading}>
          {formLoading ? "Creating..." : "Create"}
        </button>
      </form>
    </div>
  );
};
```

---

## Tour 2: Authentication Flow

### Step 1: Auth Provider

```tsx
// src/providers/authProvider.ts
import { AuthProvider } from "@refinedev/core";

export const authProvider: AuthProvider = {
  login: async ({ email, password }) => {
    // Mock login
    if (email === "demo@refine.dev" && password === "demo") {
      localStorage.setItem("auth", JSON.stringify({ email }));
      return { success: true, redirectTo: "/" };
    }
    return {
      success: false,
      error: { message: "Invalid credentials" },
    };
  },

  logout: async () => {
    localStorage.removeItem("auth");
    return { success: true, redirectTo: "/login" };
  },

  check: async () => {
    const auth = localStorage.getItem("auth");
    if (auth) {
      return { authenticated: true };
    }
    return { authenticated: false, redirectTo: "/login" };
  },

  getIdentity: async () => {
    const auth = localStorage.getItem("auth");
    if (auth) {
      return JSON.parse(auth);
    }
    return null;
  },
};
```

### Step 2: Login Page

```tsx
// src/pages/login.tsx
import { useLogin } from "@refinedev/core";
import { useState } from "react";

export const Login = () => {
  const { mutate: login, isLoading } = useLogin();
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    login({ email, password });
  };

  return (
    <div>
      <h1>Login</h1>
      <form onSubmit={handleSubmit}>
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          placeholder="Email"
        />
        <input
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          placeholder="Password"
        />
        <button type="submit" disabled={isLoading}>
          {isLoading ? "Logging in..." : "Login"}
        </button>
      </form>
      <p>Try: demo@refine.dev / demo</p>
    </div>
  );
};
```

---

## Tour 3: Real-Time Dashboard

Using Supabase for real-time updates:

```tsx
import { useList } from "@refinedev/core";

export const Dashboard = () => {
  const { data: products } = useList({
    resource: "products",
    liveMode: "auto",  // ← Auto-updates!
  });

  const { data: orders } = useList({
    resource: "orders",
    liveMode: "auto",
  });

  return (
    <div>
      <h1>Dashboard</h1>
      <div>
        <h2>Stats (Real-time)</h2>
        <p>Products: {products?.total}</p>
        <p>Orders: {orders?.total}</p>
      </div>
    </div>
  );
};
```

---

## Summary

**You've learned:**
- ✅ Build complete CRUD app
- ✅ Add authentication
- ✅ Enable real-time updates

**Next:** Try building your own app!

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
