# Providers Guide

**📅 Documented:** November 18, 2025
**🔬 Tech Stack:** [View Research](TECH_STACK_RESEARCH.md)
**👥 Audience:** Advanced developers
**⏱️ Estimated Reading:** 35-45 minutes

---

## Introduction

**Providers** are adapters between Refine and your infrastructure. They implement standard interfaces that Refine hooks call.

**🧠 Mental Model:** Providers are like translators - Refine speaks a standard language, providers translate to your specific APIs.

---

## Data Provider

The most important provider - handles all CRUD operations.

### Interface

```typescript
interface DataProvider {
  getList: <TData>(params: GetListParams) => Promise<GetListResponse<TData>>;
  getOne: <TData>(params: GetOneParams) => Promise<GetOneResponse<TData>>;
  getMany?: <TData>(params: GetManyParams) => Promise<GetManyResponse<TData>>;
  create: <TData>(params: CreateParams) => Promise<CreateResponse<TData>>;
  createMany?: <TData>(params: CreateManyParams) => Promise<CreateManyResponse<TData>>;
  update: <TData>(params: UpdateParams) => Promise<UpdateResponse<TData>>;
  updateMany?: <TData>(params: UpdateManyParams) => Promise<UpdateManyResponse<TData>>;
  deleteOne: <TData>(params: DeleteOneParams) => Promise<DeleteOneResponse<TData>>;
  deleteMany?: <TData>(params: DeleteManyParams) => Promise<DeleteManyResponse<TData>>;
  custom?: <TData>(params: CustomParams) => Promise<CustomResponse<TData>>;
  getApiUrl: () => string;
}
```

### Creating a Data Provider

```typescript
import { DataProvider } from "@refinedev/core";
import axios from "axios";

const API_URL = "https://api.example.com";

export const dataProvider: DataProvider = {
  getList: async ({ resource, pagination, filters, sorters, meta }) => {
    // Convert Refine params to API params
    const { current = 1, pageSize = 10 } = pagination ?? {};

    const query = {
      _page: current,
      _limit: pageSize,
      ...convertFilters(filters),
      ...convertSorters(sorters),
    };

    const { data, headers } = await axios.get(`${API_URL}/${resource}`, {
      params: query,
      headers: meta?.headers,
    });

    return {
      data,
      total: parseInt(headers["x-total-count"]),
    };
  },

  getOne: async ({ resource, id, meta }) => {
    const { data } = await axios.get(`${API_URL}/${resource}/${id}`, {
      headers: meta?.headers,
    });

    return { data };
  },

  create: async ({ resource, variables, meta }) => {
    const { data } = await axios.post(`${API_URL}/${resource}`, variables, {
      headers: meta?.headers,
    });

    return { data };
  },

  update: async ({ resource, id, variables, meta }) => {
    const { data } = await axios.patch(`${API_URL}/${resource}/${id}`, variables, {
      headers: meta?.headers,
    });

    return { data };
  },

  deleteOne: async ({ resource, id, meta }) => {
    const { data } = await axios.delete(`${API_URL}/${resource}/${id}`, {
      headers: meta?.headers,
    });

    return { data };
  },

  getApiUrl: () => API_URL,
};

// Helper functions
function convertFilters(filters) {
  const query = {};
  filters?.forEach((filter) => {
    if (filter.operator === "eq") {
      query[filter.field] = filter.value;
    }
    // ... handle other operators
  });
  return query;
}

function convertSorters(sorters) {
  if (!sorters || sorters.length === 0) return {};

  const { field, order } = sorters[0];
  return {
    _sort: field,
    _order: order,
  };
}
```

### Using Multiple Data Providers

```tsx
import restProvider from "@refinedev/simple-rest";
import graphqlProvider from "@refinedev/graphql";

<Refine
  dataProvider={{
    default: restProvider("https://api.example.com"),
    graphql: graphqlProvider("https://api.example.com/graphql"),
    legacy: restProvider("https://legacy-api.example.com"),
  }}
/>

// Use different providers
useList({ resource: "products", dataProviderName: "default" });
useList({ resource: "users", dataProviderName: "graphql" });
```

---

## Auth Provider

Handles authentication and authorization.

### Interface

```typescript
interface AuthProvider {
  login: (params: any) => Promise<AuthActionResponse>;
  logout: (params: any) => Promise<AuthActionResponse>;
  check: (params?: any) => Promise<CheckResponse>;
  onError: (error: any) => Promise<OnErrorResponse>;
  register?: (params: any) => Promise<AuthActionResponse>;
  forgotPassword?: (params: any) => Promise<AuthActionResponse>;
  updatePassword?: (params: any) => Promise<AuthActionResponse>;
  getPermissions?: (params?: any) => Promise<PermissionResponse>;
  getIdentity?: (params?: any) => Promise<IdentityResponse>;
}
```

### Implementation Example

```typescript
import { AuthProvider } from "@refinedev/core";

export const authProvider: AuthProvider = {
  login: async ({ email, password }) => {
    // Call auth API
    const { data } = await axios.post("/auth/login", { email, password });

    // Store token
    localStorage.setItem("token", data.token);
    localStorage.setItem("user", JSON.stringify(data.user));

    return {
      success: true,
      redirectTo: "/",
    };
  },

  logout: async () => {
    localStorage.removeItem("token");
    localStorage.removeItem("user");

    return {
      success: true,
      redirectTo: "/login",
    };
  },

  check: async () => {
    const token = localStorage.getItem("token");

    if (!token) {
      return {
        authenticated: false,
        redirectTo: "/login",
        logout: true,
      };
    }

    // Optionally verify token with server
    try {
      await axios.get("/auth/verify", {
        headers: { Authorization: `Bearer ${token}` },
      });

      return { authenticated: true };
    } catch {
      return {
        authenticated: false,
        redirectTo: "/login",
        logout: true,
      };
    }
  },

  onError: async (error) => {
    if (error?.status === 401 || error?.status === 403) {
      return {
        logout: true,
        redirectTo: "/login",
        error,
      };
    }

    return {};
  },

  getPermissions: async () => {
    const user = localStorage.getItem("user");
    if (!user) return null;

    return JSON.parse(user).permissions;
  },

  getIdentity: async () => {
    const user = localStorage.getItem("user");
    if (!user) return null;

    return JSON.parse(user);
  },
};
```

---

## Router Provider

Handles navigation and routing.

### Interface

```typescript
interface RouterProvider {
  go: (config: GoConfig) => void | Promise<void>;
  back: () => void | Promise<void>;
  parse: () => ParseResponse | Promise<ParseResponse>;
}
```

### Built-in Router Providers

```tsx
// React Router
import routerProvider from "@refinedev/react-router";

// Next.js
import routerProvider from "@refinedev/nextjs-router";

// Remix
import routerProvider from "@refinedev/remix-router";

<Refine routerProvider={routerProvider} />
```

---

## I18n Provider

Handles internationalization.

### Interface

```typescript
interface I18nProvider {
  translate: (key: string, options?: any, defaultMessage?: string) => string;
  changeLocale: (locale: string, options?: any) => Promise<any>;
  getLocale: () => string;
}
```

### Implementation Example

```typescript
import { I18nProvider } from "@refinedev/core";
import i18n from "i18next";

export const i18nProvider: I18nProvider = {
  translate: (key, options) => i18n.t(key, options),
  changeLocale: (locale) => i18n.changeLanguage(locale),
  getLocale: () => i18n.language,
};
```

---

## Notification Provider

Shows success/error/info messages.

### Interface

```typescript
interface NotificationProvider {
  open: (params: OpenNotificationParams) => void;
  close?: (key: any) => void;
}

interface OpenNotificationParams {
  key?: any;
  message: string;
  description?: string;
  type: "success" | "error" | "info" | "warning";
  cancelMutation?: () => void;  // For undoable mutations
  undoableTimeout?: number;
}
```

### Implementation Example

```typescript
import { NotificationProvider } from "@refinedev/core";
import { notification } from "antd";  // or any notification library

export const notificationProvider: NotificationProvider = {
  open: ({ key, message, type, description, cancelMutation, undoableTimeout }) => {
    if (type === "success") {
      notification.success({
        key,
        message,
        description,
      });
    } else if (type === "error") {
      notification.error({
        key,
        message,
        description,
      });
    }

    // For undoable mutations
    if (cancelMutation) {
      notification.open({
        key,
        message,
        description,
        btn: (
          <button onClick={() => {
            cancelMutation();
            notification.close(key);
          }}>
            Undo
          </button>
        ),
        duration: undoableTimeout / 1000,
      });
    }
  },

  close: (key) => notification.close(key),
};
```

---

## Access Control Provider

Handles permissions.

### Interface

```typescript
interface AccessControlProvider {
  can: (params: CanParams) => Promise<CanResponse>;
  options?: {
    buttons?: {
      enableAccessControl?: boolean;
      hideIfUnauthorized?: boolean;
    };
  };
}

interface CanParams {
  resource: string;
  action: string;
  params?: {
    resource?: IResourceItem;
    id?: BaseKey;
    [key: string]: any;
  };
}

interface CanResponse {
  can: boolean;
  reason?: string;
}
```

### Implementation Example

```typescript
import { AccessControlProvider } from "@refinedev/core";

export const accessControlProvider: AccessControlProvider = {
  can: async ({ resource, action, params }) => {
    const user = JSON.parse(localStorage.getItem("user") || "{}");

    // Admin can do anything
    if (user.role === "admin") {
      return { can: true };
    }

    // Editor can edit but not delete
    if (user.role === "editor") {
      if (action === "delete") {
        return {
          can: false,
          reason: "Only admins can delete",
        };
      }
      return { can: true };
    }

    // Viewer can only view
    if (user.role === "viewer") {
      if (["list", "show"].includes(action)) {
        return { can: true };
      }
      return {
        can: false,
        reason: "Viewers cannot modify data",
      };
    }

    return { can: false };
  },

  options: {
    buttons: {
      enableAccessControl: true,
      hideIfUnauthorized: true,  // Hide buttons if no permission
    },
  },
};
```

---

## Live Provider

Enables real-time updates.

### Interface

```typescript
interface LiveProvider {
  subscribe: (params: {
    channel: string;
    types: ("*" | "created" | "updated" | "deleted")[];
    params?: {
      ids?: BaseKey[];
      id?: BaseKey;
      meta?: any;
      pagination?: Pagination;
      sorters?: CrudSorting;
      filters?: CrudFilters;
      subscriptionType?: "useList" | "useOne" | "useMany";
      resource?: string;
    };
    callback: (event: LiveEvent) => void;
  }) => any;

  unsubscribe: (subscription: any) => void;
  publish?: (event: LiveEvent) => void;
}

interface LiveEvent {
  channel: string;
  type: "created" | "updated" | "deleted" | "*";
  payload: {
    ids?: BaseKey[];
    [key: string]: any;
  };
  date: Date;
  meta?: Record<string, any>;
}
```

### WebSocket Implementation Example

```typescript
import { LiveProvider } from "@refinedev/core";

export const liveProvider: LiveProvider = {
  subscribe: ({ channel, types, callback }) => {
    const ws = new WebSocket(`wss://api.example.com/live`);

    ws.onopen = () => {
      ws.send(JSON.stringify({
        action: "subscribe",
        channel,
        types,
      }));
    };

    ws.onmessage = (message) => {
      const event = JSON.parse(message.data);
      callback(event);
    };

    return ws;  // Return subscription handle
  },

  unsubscribe: (ws) => {
    ws.close();
  },

  publish: (event) => {
    // Optional: publish events
    fetch("https://api.example.com/events", {
      method: "POST",
      body: JSON.stringify(event),
    });
  },
};

// Usage
<Refine
  liveProvider={liveProvider}
  options={{ liveMode: "auto" }}  // auto | manual | off
/>
```

---

## Audit Log Provider

Logs all operations for auditing.

### Interface

```typescript
interface AuditLogProvider {
  create: (params: {
    resource: string;
    action: string;
    data?: any;
    author?: any;
    meta?: Record<string, any>;
  }) => Promise<any>;

  get: (params: {
    resource: string;
    action?: string;
    meta?: Record<string, any>;
    author?: any;
  }) => Promise<any>;

  update: (params: {
    id: BaseKey;
    name: string;
  }) => Promise<any>;
}
```

### Implementation Example

```typescript
import { AuditLogProvider } from "@refinedev/core";

export const auditLogProvider: AuditLogProvider = {
  create: async ({ resource, action, data, author, meta }) => {
    await axios.post("/audit-logs", {
      resource,
      action,
      data,
      author: author || getCurrentUser(),
      timestamp: new Date(),
      meta,
    });
  },

  get: async ({ resource, action }) => {
    const { data } = await axios.get("/audit-logs", {
      params: { resource, action },
    });
    return data;
  },

  update: async ({ id, name }) => {
    await axios.patch(`/audit-logs/${id}`, { name });
  },
};
```

---

## Summary

**Provider Types:**
1. **Data Provider** - CRUD operations (required)
2. **Auth Provider** - Authentication (optional)
3. **Router Provider** - Navigation (required for routing)
4. **I18n Provider** - Translations (optional)
5. **Notification Provider** - Messages (optional)
6. **Access Control Provider** - Permissions (optional)
7. **Audit Log Provider** - Logging (optional)
8. **Live Provider** - Real-time (optional)

**Creating Providers:**
- Implement the interface
- Handle Refine's standard format
- Translate to your API format
- Return standardized responses

---

## Next Steps

**See data providers:**
→ [Data Providers Guide](DATA_PROVIDERS_GUIDE.md)

**Learn routing:**
→ [Routing Guide](ROUTING_GUIDE.md)

**Build features:**
→ [How-To Guide](HOW_TO_GUIDE.md)

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
