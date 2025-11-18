# Project Structure Guide

**📅 Documented:** November 18, 2025
**🔬 Tech Stack:** [View Research](TECH_STACK_RESEARCH.md)
**👥 Audience:** All developers
**⏱️ Estimated Reading:** 15-20 minutes

---

## Overview

Refine is a **monorepo** containing 40+ packages and 280+ examples managed by Lerna and pnpm workspaces.

**🧠 Mental Model:** Think of it as a city with different districts (packages, examples, docs) sharing infrastructure (node_modules, build tools).

---

## Repository Root

```
refine/
├── packages/              # 40 publishable npm packages
├── examples/              # 280+ example applications
├── documentation/         # Docusaurus documentation site
├── docs/                  # Additional documentation
├── cypress/               # E2E test suite
├── scripts/               # Build and utility scripts
├── patches/               # pnpm patches
├── .github/               # GitHub Actions CI/CD
├── package.json           # Root package.json
├── pnpm-workspace.yaml    # Workspace configuration
├── lerna.json             # Lerna configuration
├── nx.json                # Nx build configuration
├── tsconfig.json          # TypeScript configuration
└── biome.json             # Biome linter/formatter config
```

---

## Packages Directory

### Core Packages

**`packages/core/`** - **@refinedev/core**
- The heart of Refine
- All hooks, contexts, provider interfaces
- Zero UI dependencies
- [Source →](../../packages/core/)

```
packages/core/
├── src/
│   ├── components/     # Core components (Refine, Authenticated, etc.)
│   ├── contexts/       # React contexts (14 providers)
│   ├── hooks/          # 30+ React hooks
│   └── definitions/    # TypeScript types and utilities
├── package.json
└── tsconfig.json
```

### UI Integration Packages

**`packages/antd/`** - **@refinedev/antd**
- Ant Design integration
- 50+ components
- [Ant Design Docs →](https://ant.design/)

**`packages/mui/`** - **@refinedev/mui**
- Material UI integration
- [MUI Docs →](https://mui.com/)

**`packages/mantine/`** - **@refinedev/mantine**
- Mantine integration
- [Mantine Docs →](https://mantine.dev/)

**`packages/chakra-ui/`** - **@refinedev/chakra-ui**
- Chakra UI integration
- [Chakra Docs →](https://chakra-ui.com/)

### Router Integration Packages

**`packages/react-router/`** - **@refinedev/react-router**
- React Router v6+ integration
- ✅ CURRENT (Nov 2025): React Router is industry standard

**`packages/nextjs-router/`** - **@refinedev/nextjs-router**
- Next.js App Router and Pages Router support
- ✅ CURRENT (Nov 2025): Next.js 15 App Router is 2025 standard

**`packages/remix-router/`** - **@refinedev/remix-router**
- Remix integration

### Data Provider Packages

**`packages/simple-rest/`** - **@refinedev/simple-rest**
- Simple REST API provider
- Good starting point

**`packages/graphql/`** - **@refinedev/graphql**
- GraphQL data provider

**`packages/supabase/`** - **@refinedev/supabase**
- Supabase integration (PostgreSQL + Auth + Storage)

**`packages/strapi-v4/`** - **@refinedev/strapi-v4**
- Strapi CMS v4 integration

**`packages/nestjs-query/`** - **@refinedev/nestjs-query**
- NestJS Query integration

**`packages/airtable/`** - **@refinedev/airtable**
- Airtable integration

**`packages/hasura/`** - **@refinedev/hasura**
- Hasura GraphQL integration

**`packages/medusa/`** - **@refinedev/medusa**
- Medusa e-commerce integration

### Utility Packages

**`packages/cli/`** - **@refinedev/cli**
- Command-line interface for Refine
- Code generation, project scaffolding

**`packages/create-refine-app/`** - **create-refine-app**
- Project initializer (`npm create refine-app`)

**`packages/devtools/`** - **@refinedev/devtools**
- Browser DevTools for Refine

**`packages/react-hook-form/`** - **@refinedev/react-hook-form**
- React Hook Form adapter

**`packages/react-table/`** - **@refinedev/react-table**
- React Table adapter

**`packages/kbar/`** - **@refinedev/kbar**
- Command palette integration

**`packages/inferencer/`** - **@refinedev/inferencer**
- Auto-generate CRUD pages from API

### Internal Packages

**`packages/devtools-internal/`**
- Internal DevTools utilities

**`packages/devtools-shared/`**
- Shared DevTools code

**`packages/shared/`**
- Shared utilities across packages

---

## Examples Directory

### Example Organization

Examples follow naming convention: `{category}-{framework}-{feature}`

**Categories:**
- `base-*` - Basic examples
- `auth-*` - Authentication examples
- `data-provider-*` - Data provider examples
- `form-*` - Form examples
- `table-*` - Table examples
- `customization-*` - Customization examples
- `blog-*` - Blog post examples
- `app-*` - Complete applications

### Key Examples

**Beginner:**
- `base-headless` - Pure Refine without UI framework
- `base-antd` - Refine + Ant Design
- `base-mui` - Refine + Material UI

**Authentication:**
- `auth-antd` - Basic auth with Ant Design
- `auth-auth0` - Auth0 integration
- `auth-google-login` - Google OAuth

**Real Applications:**
- `finefoods-antd` - Complete admin panel
- `finefoods-client` - Customer storefront
- `app-crm-minimal` - Minimal CRM

[Browse all examples →](../../examples/)

---

## Documentation Directory

**`documentation/`** - Docusaurus site
- Official documentation website
- Built with Docusaurus 2.4.0
- 🚨 Behind version 3.9 ([details](TECH_STACK_RESEARCH.md#docusaurus---v240))

```
documentation/
├── docs/           # Documentation MDX files
├── blog/           # Blog posts
├── src/            # Custom pages and components
├── static/         # Static assets
└── package.json
```

**`docs/learning/`** - Learning materials (this directory!)
- Getting started guides
- Architecture documentation
- Code tours and exercises

---

## Configuration Files

### Root Level

**`package.json`** - Root package
```json
{
  "name": "refinedev",
  "private": true,
  "scripts": {
    "build": "lerna run build --scope @refinedev/core",
    "build:all": "lerna run build --scope @refinedev/*",
    "test": "lerna run test --stream",
    // ... more scripts
  },
  "packageManager": "pnpm@9.4.0"
}
```

**`pnpm-workspace.yaml`** - Workspace config
```yaml
packages:
  - 'packages/*'
  - 'examples/*'
```

**`lerna.json`** - Monorepo config
```json
{
  "packages": ["packages/*", "examples/*"],
  "version": "4.0.0",
  "npmClient": "pnpm"
}
```

**`nx.json`** - Build optimization
- Nx provides caching and task orchestration
- 🚨 Project uses Nx 18.2.2, latest is 22.x ([details](TECH_STACK_RESEARCH.md#nx---v1822))

**`tsconfig.json`** - TypeScript config
- Root config extends `tsconfig.build.json`
- Individual packages have their own tsconfig

**`biome.json`** - Linter/formatter config
- Replaces ESLint + Prettier
- 🚨 Project uses Biome 1.7.3, latest is 2.0 ([details](TECH_STACK_RESEARCH.md#biome---v173))

---

## Package Structure Template

Each package follows this structure:

```
packages/{package-name}/
├── src/                   # Source code
│   ├── index.ts          # Main export
│   ├── components/       # React components (if any)
│   ├── hooks/            # React hooks (if any)
│   └── types.ts          # TypeScript types
├── dist/                  # Build output (gitignored)
│   ├── index.js          # CJS bundle
│   ├── index.mjs         # ESM bundle
│   └── index.d.ts        # Type declarations
├── package.json           # Package configuration
├── tsconfig.json          # TypeScript configuration
├── README.md              # Package documentation
└── CHANGELOG.md           # Change history
```

### Package.json Structure

```json
{
  "name": "@refinedev/core",
  "version": "5.0.6",
  "main": "dist/index.cjs",
  "module": "dist/index.mjs",
  "types": "dist/index.d.ts",
  "exports": {
    ".": {
      "import": "./dist/index.mjs",
      "require": "./dist/index.cjs",
      "types": "./dist/index.d.ts"
    }
  },
  "scripts": {
    "build": "tsup && node ../shared/generate-declarations.js",
    "dev": "tsup --watch",
    "test": "vitest run"
  },
  "peerDependencies": {
    "react": "^18.0.0 || ^19.0.0",
    "@tanstack/react-query": "^5.81.5"
  }
}
```

---

## Build System

### Build Tools

**tsup** - TypeScript bundler
- 🚨 No longer maintained, should migrate to tsdown ([details](TECH_STACK_RESEARCH.md#tsup---v670))
- Powered by esbuild
- Generates CJS, ESM, and type declarations

**Lerna** - Monorepo management
- Handles versioning and publishing
- 🚨 Project uses v8.1.2, latest is v9.0.1 ([details](TECH_STACK_RESEARCH.md#lerna---v812))

**Nx** - Build optimization
- Caching and task orchestration
- 🚨 Project uses v18.2.2, latest is v22.x ([details](TECH_STACK_RESEARCH.md#nx---v1822))

### Build Commands

```bash
# Build single package
pnpm build --scope @refinedev/core

# Build all packages
pnpm build:all

# Watch mode (development)
cd packages/core
pnpm dev
```

---

## Testing Structure

### Test Files Location

Tests are co-located with source files:

```
src/
├── hooks/
│   ├── data/
│   │   ├── useList/
│   │   │   ├── index.ts
│   │   │   └── index.spec.ts    # ← Test file
```

### Test Stack

- **Vitest** - Test runner
- **@testing-library/react** - React component testing
- **Cypress** - E2E testing

🚨 **Version Gaps:**
- Vitest 2.1.8 (behind 4.0.8)
- Cypress 13.6.3 (behind 15.6.0)

[Details →](TECH_STACK_RESEARCH.md#testing--quality)

---

## CI/CD Structure

**`.github/workflows/`** - GitHub Actions

Key workflows:
- `test.yml` - Run tests on PR
- `build.yml` - Build all packages
- `release.yml` - Publish to npm
- `examples.yml` - Test examples

---

## Git Structure

**Branches:**
- `master` - Main branch
- `next` - Next version development
- Feature branches: `feat/*`
- Bug fixes: `fix/*`

**`.gitignore`** - Ignored files
```
node_modules/
dist/
.next/
build/
*.log
.env.local
```

---

## Navigating the Codebase

### Finding Things

**Find a package:**
```bash
ls packages/           # List all packages
cd packages/core       # Navigate to core
```

**Find examples:**
```bash
ls examples/ | grep auth     # All auth examples
ls examples/ | grep antd     # All Ant Design examples
```

**Find source code:**
```bash
# Find hook implementation
find packages/core/src -name "useList*"

# Find all hooks
ls packages/core/src/hooks/
```

**Search code:**
```bash
# Search for a function
grep -r "useList" packages/core/src/

# Search for a type
grep -r "DataProvider" packages/core/src/
```

### Opening in Editor

**VS Code:**
```bash
# Open entire monorepo
code .

# Open specific package
code packages/core

# Open specific example
code examples/base-headless
```

**Workspace settings (`.vscode/settings.json`):**
- TypeScript version
- ESLint/Biome settings
- Recommended extensions

---

## Common Workflows

### Adding a New Package

1. Create directory in `packages/`
2. Add `package.json` with workspace dependencies
3. Add to `lerna.json` if needed
4. Run `pnpm install`

### Adding a New Example

1. Create directory in `examples/`
2. Follow naming convention
3. Add `package.json`
4. Run `pnpm install`
5. Add to `.gitignore` if needed

### Working on a Package

```bash
# 1. Navigate to package
cd packages/core

# 2. Watch for changes
pnpm dev

# 3. In another terminal, run example
cd examples/base-headless
pnpm dev

# 4. Changes to core rebuild automatically!
```

---

## Dependencies

### Dependency Types

**Workspace Dependencies:**
```json
{
  "dependencies": {
    "@refinedev/core": "workspace:*"
  }
}
```

**External Dependencies:**
```json
{
  "dependencies": {
    "react": "^19.1.0"
  }
}
```

**Peer Dependencies:**
```json
{
  "peerDependencies": {
    "react": "^18.0.0 || ^19.0.0"
  }
}
```

### Managing Dependencies

```bash
# Add to specific package
pnpm --filter @refinedev/core add lodash

# Add to all packages
pnpm -r add lodash

# Update all dependencies
pnpm update -r
```

---

## Summary

**Refine Repository Structure:**

```
40 packages + 280 examples + Docs + Tests = Monorepo
```

**Key Directories:**
- `packages/core/` - The brain of Refine
- `packages/{ui}/` - UI integrations
- `packages/{router}/` - Router integrations
- `packages/{data}/` - Data providers
- `examples/` - Learning by example

**Build Tools:**
- pnpm - Package manager
- Lerna - Monorepo management
- Nx - Build optimization
- tsup - TypeScript bundling

---

## Next Steps

**Understand the code:**
→ [Architecture Overview](ARCHITECTURE_OVERVIEW.md)

**Learn the stack:**
→ [Tech Stack Guide](TECH_STACK_GUIDE.md)

**Start developing:**
→ [Development Workflow](DEVELOPMENT_WORKFLOW.md)

**Build something:**
→ [Getting Started](GETTING_STARTED.md)

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
