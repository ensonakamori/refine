# Data Providers Guide

**📅 Documented:** November 18, 2025
**🔬 Tech Stack:** [View Research](TECH_STACK_RESEARCH.md)
**👥 Audience:** Developers connecting to APIs
**⏱️ Estimated Reading:** 25-30 minutes

---

## Available Data Providers

Refine supports many backend services out of the box:

### REST APIs

**@refinedev/simple-rest**
```bash
npm install @refinedev/simple-rest
```
```tsx
import dataProvider from "@refinedev/simple-rest";

<Refine dataProvider={dataProvider("https://api.example.com")} />
```

**Best for:** Standard REST APIs following common conventions

---

### GraphQL

**@refinedev/graphql**
```bash
npm install @refinedev/graphql graphql-request graphql
```
```tsx
import { GraphQLClient } from "graphql-request";
import graphqlDataProvider from "@refinedev/graphql";

const client = new GraphQLClient("https://api.example.com/graphql");

<Refine
  dataProvider={graphqlDataProvider(client)}
/>
```

---

### Supabase

**@refinedev/supabase**
```bash
npm install @refinedev/supabase @supabase/supabase-js
```
```tsx
import { createClient } from "@supabase/supabase-js";
import { dataProvider, liveProvider } from "@refinedev/supabase";

const supabaseClient = createClient(SUPABASE_URL, SUPABASE_KEY);

<Refine
  dataProvider={dataProvider(supabaseClient)}
  liveProvider={liveProvider(supabaseClient)}  // Real-time!
/>
```

---

### Strapi

**@refinedev/strapi-v4**
```bash
npm install @refinedev/strapi-v4
```
```tsx
import { DataProvider } from "@refinedev/strapi-v4";

<Refine
  dataProvider={DataProvider("https://api.example.com")}
  authProvider={AuthHelper("https://api.example.com")}
/>
```

---

### NestJS Query

**@refinedev/nestjs-query**
```bash
npm install @refinedev/nestjs-query graphql-request
```

---

### Hasura

**@refinedev/hasura**
```bash
npm install @refinedev/hasura graphql-request
```

---

### Airtable

**@refinedev/airtable**
```bash
npm install @refinedev/airtable
```

---

### Medusa

**@refinedev/medusa**
```bash
npm install @refinedev/medusa @medusajs/medusa-js
```

---

## Creating Custom Data Provider

For custom APIs or unsupported backends:

```typescript
import { DataProvider, HttpError } from "@refinedev/core";
import axios, { AxiosInstance } from "axios";

export const customDataProvider = (
  apiUrl: string,
  httpClient: AxiosInstance = axios
): DataProvider => ({
  getList: async ({ resource, pagination, filters, sorters, meta }) => {
    const url = `${apiUrl}/${resource}`;

    // Convert Refine params to your API format
    const { current = 1, pageSize = 10, mode = "server" } = pagination ?? {};

    const queryFilters = filters?.map((filter) => ({
      [`${filter.field}_${filter.operator}`]: filter.value,
    }));

    const query = {
      page: current,
      limit: pageSize,
      ...Object.assign({}, ...queryFilters),
    };

    // Sorting
    if (sorters && sorters.length > 0) {
      query.sort = sorters[0].field;
      query.order = sorters[0].order;
    }

    try {
      const { data, headers } = await httpClient.get(url, { params: query });

      return {
        data: data.items,
        total: parseInt(headers["x-total-count"]) || data.total,
      };
    } catch (error) {
      const httpError: HttpError = {
        message: error.response?.data?.message || error.message,
        statusCode: error.response?.status,
      };
      throw httpError;
    }
  },

  getOne: async ({ resource, id }) => {
    const url = `${apiUrl}/${resource}/${id}`;

    const { data } = await httpClient.get(url);

    return { data };
  },

  create: async ({ resource, variables }) => {
    const url = `${apiUrl}/${resource}`;

    const { data } = await httpClient.post(url, variables);

    return { data };
  },

  update: async ({ resource, id, variables }) => {
    const url = `${apiUrl}/${resource}/${id}`;

    const { data } = await httpClient.patch(url, variables);

    return { data };
  },

  deleteOne: async ({ resource, id }) => {
    const url = `${apiUrl}/${resource}/${id}`;

    const { data } = await httpClient.delete(url);

    return { data };
  },

  getApiUrl: () => apiUrl,

  // Optional methods
  getMany: async ({ resource, ids }) => {
    const { data } = await httpClient.get(`${apiUrl}/${resource}`, {
      params: { ids: ids.join(",") },
    });

    return { data };
  },

  custom: async ({ url, method, filters, sorters, payload, query, headers }) => {
    const { data } = await httpClient({
      url: `${apiUrl}${url}`,
      method,
      data: payload,
      params: query,
      headers,
    });

    return { data };
  },
});
```

---

## Multiple Data Providers

Use different backends for different resources:

```tsx
<Refine
  dataProvider={{
    default: simpleRestProvider("https://api.example.com"),
    cms: strapiProvider("https://cms.example.com"),
    analytics: customProvider("https://analytics.example.com"),
  }}
/>

// Use different providers
useList({ resource: "products", dataProviderName: "default" });
useList({ resource: "pages", dataProviderName: "cms" });
useCustom({ url: "/events", dataProviderName: "analytics" });
```

---

## Provider Comparison

| Provider | Best For | Features |
|----------|----------|----------|
| **simple-rest** | Standard REST APIs | Simple, flexible |
| **GraphQL** | GraphQL APIs | Type-safe, efficient |
| **Supabase** | PostgreSQL + Auth | Real-time, Auth built-in |
| **Strapi** | Headless CMS | Content management |
| **Hasura** | GraphQL + PostgreSQL | Auto-generated GraphQL |
| **NestJS Query** | NestJS backends | TypeScript end-to-end |
| **Airtable** | No-code databases | Quick prototyping |
| **Medusa** | E-commerce | Commerce features |
| **Custom** | Any API | Full control |

---

## Summary

**Refine supports:**
- ✅ 8+ official data providers
- ✅ Custom provider creation
- ✅ Multiple providers simultaneously
- ✅ TypeScript-first

**Choose based on:**
- Your backend technology
- Features needed (real-time, auth, etc.)
- Team expertise

---

## Next Steps

→ [Providers Guide](PROVIDERS_GUIDE.md)
→ [How-To Guide](HOW_TO_GUIDE.md)

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
