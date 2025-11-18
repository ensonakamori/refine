# Technology Stack Guide

**📅 Documented:** November 18, 2025
**🔬 Full Research:** [TECH_STACK_RESEARCH.md](TECH_STACK_RESEARCH.md)
**👥 Audience:** Developers wanting technical details
**⏱️ Estimated Reading:** 25-35 minutes

---

## Overview

This guide explains **how Refine uses each technology** in its stack. For version analysis and update recommendations, see [TECH_STACK_RESEARCH.md](TECH_STACK_RESEARCH.md).

---

## Core Stack

### React 19.1.0

**Role:** UI library foundation

**Version Status:** ⚠️ Slightly behind 19.2.0
[Full analysis →](TECH_STACK_RESEARCH.md#react---v1910)

**How Refine Uses React:**

```tsx
// Refine is 100% React - hooks, contexts, components
import { useList } from "@refinedev/core";

function Products() {
  // Standard React hook
  const { data, isLoading } = useList({
    resource: "products"
  });

  // Standard React rendering
  return <div>{/* ... */}</div>;
}
```

**Key React Features Used:**
- **Hooks** - All Refine hooks follow React hooks rules
- **Context API** - Provider pattern implementation
- **Suspense** - Supported via TanStack Query
- **Server Components** - Supported in Next.js integration

✅ **CURRENT (Nov 2025):** React 19.x patterns are modern and current

**React 19 Features in Refine:**

```tsx
// Actions (React 19)
<form action={async (formData) => {
  await createProduct(formData);
}}>

// use() API (React 19)
const data = use(dataPromise);

// Server Components (React 19 + Next.js)
async function ServerProductList() {
  const data = await fetch('/api/products');
  return <List data={data} />;
}
```

**Learning Resources:**
- [React Docs](https://react.dev/)
- [React 19 Release](https://react.dev/blog/2025/10/01/react-19-2)

---

### TypeScript 5.8.3

**Role:** Type safety and developer experience

**Version Status:** ⚠️ Behind 5.9.3
[Full analysis →](TECH_STACK_RESEARCH.md#typescript---v583)

**How Refine Uses TypeScript:**

**Type Inference:**
```typescript
// Refine hooks have excellent type inference
const { data } = useList<IProduct>({ resource: "products" });
//    ^? data: GetListResponse<IProduct> | undefined

// TypeScript knows the shape
data?.data.map(product => product.name);
//                        ^? string
```

**Generic Hooks:**
```typescript
interface IProduct {
  id: number;
  name: string;
  price: number;
}

// Type flows through
const { mutate } = useCreate<IProduct>();

mutate({
  resource: "products",
  values: {
    name: "Phone",      // ✅ TypeScript validates
    price: 999,         // ✅ TypeScript validates
    invalid: "field"    // ❌ TypeScript error!
  }
});
```

**Provider Interfaces:**
```typescript
// All providers are fully typed interfaces
import type { DataProvider } from "@refinedev/core";

const myProvider: DataProvider = {
  getList: async <TData>({ resource }) => {
    // TypeScript ensures correct return type
    return {
      data: [] as TData[],
      total: 0,
    };
  },
  // ... TypeScript enforces all methods
};
```

**Key TypeScript Features Used:**
- Generics with inference
- Conditional types
- Utility types (`Omit`, `Pick`, `Partial`)
- Template literal types
- Type guards

**Learning Resources:**
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [TypeScript 5.8 Release](https://devblogs.microsoft.com/typescript/announcing-typescript-5-8/)

---

### TanStack Query 5.81.5

**Role:** Server state management

**Version Status:** ✅ Modern v5 (slightly behind 5.90.7)
[Full analysis →](TECH_STACK_RESEARCH.md#tanstack-query-react-query---v5815)

**Why TanStack Query?**

**Without TanStack Query (manual approach):**
```tsx
function ProductList() {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    setLoading(true);
    fetch('/api/products')
      .then(res => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, []);

  // Manual caching, refetching, error handling...
}
```

**With TanStack Query (Refine approach):**
```tsx
function ProductList() {
  const { data, isLoading, error } = useList({
    resource: "products"
  });

  // Automatic caching, refetching, error handling!
}
```

**How Refine Uses TanStack Query:**

```typescript
// Refine hooks wrap TanStack Query
export const useList = <TData>({
  resource,
  // ... other params
}) => {
  const queryResult = useQuery({
    queryKey: keys().data().resource(resource).action("list").get(),
    queryFn: () => dataProvider.getList({ resource }),
    // ... Refine adds smart defaults
  });

  return queryResult;
};
```

**Features Refine Leverages:**
- ✅ Automatic background refetching
- ✅ Caching with intelligent invalidation
- ✅ Optimistic updates
- ✅ Request deduplication
- ✅ Parallel queries
- ✅ Infinite scrolling
- ✅ Suspense support

**TanStack Query v5 Changes:**
```typescript
// v4 (old)
isLoading    // Was called this
useErrorBoundary: true

// v5 (current in Refine)
isPending    // Renamed for clarity
throwOnError: true  // Renamed

// Suspense (now stable in v5)
const { data } = useSuspenseQuery(/* ... */);
```

**Learning Resources:**
- [TanStack Query Docs](https://tanstack.com/query/latest)
- [Migration to v5](https://tanstack.com/query/latest/docs/framework/react/guides/migrating-to-v5)

---

### Node.js

**Role:** Runtime environment

**Version Status:** 🚨 Minimum requirement (>=18) is past EOL
[Full analysis →](TECH_STACK_RESEARCH.md#nodejs---18)

**Recommended:** Node.js 24.x LTS

**Features Used:**
- ES Modules
- Package exports
- Modern APIs

**Learning Resources:**
- [Node.js Docs](https://nodejs.org/docs/latest/api/)
- [Node.js 24 Release](https://nodejs.org/en/blog/release/v24.11.0)

---

## Monorepo Tools

### pnpm 9.4.0

**Role:** Package manager

**Version Status:** ✅ Current
[Full analysis →](TECH_STACK_RESEARCH.md#pnpm---v940)

**Why pnpm over npm/yarn?**

**Disk Space:**
```
npm:  1000 MB for 10 projects
yarn: 1000 MB for 10 projects
pnpm: 300 MB for 10 projects (shared store!)
```

**How Refine Uses pnpm:**

**Workspaces:**
```yaml
# pnpm-workspace.yaml
packages:
  - 'packages/*'
  - 'examples/*'
```

**Workspace Dependencies:**
```json
{
  "dependencies": {
    "@refinedev/core": "workspace:*"
  }
}
```

**Commands:**
```bash
# Install all dependencies
pnpm install

# Add to specific package
pnpm --filter @refinedev/core add lodash

# Run script in all packages
pnpm -r run build
```

**Learning Resources:**
- [pnpm Docs](https://pnpm.io/)
- [Workspace Guide](https://pnpm.io/workspaces)

---

### Lerna 8.1.2

**Role:** Monorepo versioning and publishing

**Version Status:** 🚨 Behind 9.0.1
[Full analysis →](TECH_STACK_RESEARCH.md#lerna---v812)

**How Refine Uses Lerna:**

**Version Management:**
```bash
# Update version for all packages
lerna version

# Publish all packages
lerna publish
```

**Selective Building:**
```bash
# Build only core
lerna run build --scope @refinedev/core

# Build all packages
lerna run build --scope @refinedev/*
```

**Learning Resources:**
- [Lerna Docs](https://lerna.js.org/)
- [Lerna with pnpm](https://lerna.js.org/docs/getting-started)

---

### Nx 18.2.2

**Role:** Build optimization

**Version Status:** 🚨 Behind 22.x
[Full analysis →](TECH_STACK_RESEARCH.md#nx---v1822)

**How Nx Helps:**

**Caching:**
```bash
# First build: 5 minutes
pnpm build:all

# Second build (cached): 5 seconds!
pnpm build:all
```

**Task Orchestration:**
```json
// nx.json
{
  "tasksRunnerOptions": {
    "default": {
      "runner": "nx/tasks-runners/default",
      "options": {
        "cacheableOperations": ["build", "test"]
      }
    }
  }
}
```

**Learning Resources:**
- [Nx Docs](https://nx.dev/)
- [Nx with Lerna](https://nx.dev/recipes/adopting-nx/lerna-and-nx)

---

## Build Tools

### tsup 6.7.0

**Role:** TypeScript bundler

**Version Status:** 🚨 No longer maintained
[Full analysis →](TECH_STACK_RESEARCH.md#tsup---v670)

**How Refine Uses tsup:**

```typescript
// packages/core/package.json
{
  "scripts": {
    "build": "tsup && node ../shared/generate-declarations.js"
  }
}
```

**What tsup does:**
1. Bundles TypeScript → JavaScript
2. Generates CJS and ESM outputs
3. Tree-shaking
4. Minification

**Output:**
```
dist/
├── index.cjs       # CommonJS
├── index.mjs       # ES Modules
└── index.d.ts      # Type declarations
```

⚠️ **Migration Needed:** Should migrate to tsdown (tsup successor)

---

### Biome 1.7.3

**Role:** Linter and formatter

**Version Status:** 🚨 Behind 2.0
[Full analysis →](TECH_STACK_RESEARCH.md#biome---v173)

**Replaces:**
- ESLint (linting)
- Prettier (formatting)

**How Refine Uses Biome:**

```bash
# Check code
pnpm biome check .

# Format code
pnpm biome format --write .

# Lint code
pnpm biome lint .
```

**Configuration:**
```json
// biome.json
{
  "formatter": {
    "enabled": true,
    "indentStyle": "space",
    "indentWidth": 2
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true
    }
  }
}
```

**Learning Resources:**
- [Biome Docs](https://biomejs.dev/)

---

## Testing Stack

### Vitest 2.1.8

**Role:** Unit/integration test runner

**Version Status:** 🚨 Behind 4.0.8
[Full analysis →](TECH_STACK_RESEARCH.md#vitest---v218)

**Why Vitest?**
- ⚡ Faster than Jest
- ✅ Vite-powered
- ✅ Jest-compatible API
- ✅ TypeScript support

**How Refine Uses Vitest:**

```typescript
// packages/core/src/hooks/data/useList/index.spec.ts

import { renderHook } from "@testing-library/react";
import { useList } from "./index";

describe("useList", () => {
  it("fetches data", async () => {
    const { result } = renderHook(() =>
      useList({ resource: "products" })
    );

    await waitFor(() => {
      expect(result.current.data).toBeDefined();
    });
  });
});
```

**Learning Resources:**
- [Vitest Docs](https://vitest.dev/)
- [Migration from Jest](https://vitest.dev/guide/migration.html)

---

### Cypress 13.6.3

**Role:** E2E testing

**Version Status:** 🚨 Behind 15.6.0
[Full analysis →](TECH_STACK_RESEARCH.md#cypress---v1363)

**How Refine Uses Cypress:**

```javascript
// cypress/e2e/example.cy.ts

describe("Product CRUD", () => {
  it("creates a product", () => {
    cy.visit("/products/create");
    cy.get('input[name="name"]').type("New Product");
    cy.get('button[type="submit"]').click();
    cy.url().should("include", "/products");
  });
});
```

**Learning Resources:**
- [Cypress Docs](https://docs.cypress.io/)

---

## Documentation

### Docusaurus 2.4.0

**Role:** Documentation website

**Version Status:** 🚨 Behind 3.9
[Full analysis →](TECH_STACK_RESEARCH.md#docusaurus---v240)

**How Refine Uses Docusaurus:**

```bash
cd documentation
pnpm dev  # Start docs site
```

**Structure:**
```
documentation/
├── docs/          # Markdown docs
├── blog/          # Blog posts
├── src/           # React components
└── docusaurus.config.js
```

**Learning Resources:**
- [Docusaurus Docs](https://docusaurus.io/)

---

## Version Control

### Git + GitHub

**Branching Strategy:**
- `master` - Production
- `next` - Development
- Feature branches

**Hooks (Husky):**
```bash
# Pre-commit: lint staged files
# Pre-push: run tests
```

---

## CI/CD

### GitHub Actions

**Workflows:**
- Test on PR
- Build on merge
- Publish on release
- E2E tests

```yaml
# .github/workflows/test.yml
name: Test
on: [pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: pnpm/action-setup@v2
      - run: pnpm install
      - run: pnpm test:all
```

---

## Summary

**Refine Tech Stack:**

```
React 19 + TypeScript 5.8 + TanStack Query 5
    ↓
Monorepo (pnpm + Lerna + Nx)
    ↓
Build (tsup + Biome)
    ↓
Test (Vitest + Cypress)
    ↓
Publish (npm)
```

**Status Overview:**
- ✅ Core stack is modern (React, TS, TanStack Query)
- ⚠️ Several tools need updates (Lerna, Nx, Vitest, Cypress)
- 🚨 Node 18 min requirement is past EOL

[Full version analysis →](TECH_STACK_RESEARCH.md)

---

## Next Steps

**Understand architecture:**
→ [Architecture Overview](ARCHITECTURE_OVERVIEW.md)

**See data flow:**
→ [Data Flow Guide](DATA_FLOW_GUIDE.md)

**Start developing:**
→ [Development Workflow](DEVELOPMENT_WORKFLOW.md)

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
