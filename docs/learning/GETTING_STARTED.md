# Getting Started with Refine

**📅 Documented:** November 18, 2025
**🔬 Tech Stack Status:** [View Research](TECH_STACK_RESEARCH.md)
**👥 Target Audience:** Developers new to Refine
**⏱️ Estimated Time:** 30-60 minutes

---

## Welcome!

This guide will help you:
- ✅ Set up your development environment
- ✅ Understand the repository structure
- ✅ Run your first Refine application
- ✅ Explore example applications
- ✅ Know where to go next

**💡 Prerequisites:** Basic knowledge of React, TypeScript, and npm/pnpm

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Development Environment Setup](#development-environment-setup)
3. [Clone and Install](#clone-and-install)
4. [Understanding the Monorepo](#understanding-the-monorepo)
5. [Running Your First Example](#running-your-first-example)
6. [Exploring Examples](#exploring-examples)
7. [Development Workflow](#development-workflow)
8. [Common Issues](#common-issues)
9. [Next Steps](#next-steps)

---

## Prerequisites

### Required Knowledge

**You should be comfortable with:**
- ✅ JavaScript/TypeScript fundamentals
- ✅ React basics (components, hooks, props)
- ✅ npm/pnpm package managers
- ✅ Command line basics
- ✅ Git basics

**Nice to have:**
- 🎯 React Router or Next.js experience
- 🎯 REST API concepts
- 🎯 State management (TanStack Query helps!)

### Required Software

**Install these before continuing:**

#### 1. Node.js (v24 LTS Recommended)

⚠️ **IMPORTANT:** The project currently requires Node.js >= 18, but **Node.js 18 reached End of Life in April 2025**. For security and compatibility, use Node.js 24 LTS.

```bash
# Check your Node.js version
node --version

# Should output: v24.x.x or v22.x.x
```

**Installation:**
- 🌐 [Node.js Official Site](https://nodejs.org/) - Download installer
- 🔧 [nvm](https://github.com/nvm-sh/nvm) - Version manager (recommended)
  ```bash
  # With nvm
  nvm install 24
  nvm use 24
  ```

✅ **CURRENT (Nov 2025):** Node.js 24.11.0 "Krypton" LTS

[Read more about Node.js versions →](TECH_STACK_RESEARCH.md#nodejs---18)

#### 2. pnpm (v9.x)

Refine uses **pnpm** as its package manager for better performance and disk space efficiency.

```bash
# Install pnpm globally
npm install -g pnpm@latest

# Check version
pnpm --version

# Should output: 9.x.x
```

**Why pnpm?**
- ⚡ Faster installs than npm/yarn
- 💾 Saves disk space (shared dependencies)
- 🎯 Stricter dependency resolution
- ✅ Perfect for monorepos

[pnpm Documentation →](https://pnpm.io/)

#### 3. Git

```bash
# Check if Git is installed
git --version

# Should output: git version 2.x.x
```

[Download Git →](https://git-scm.com/)

#### 4. Code Editor

**Recommended:** [VS Code](https://code.visualstudio.com/) with extensions:
- ESLint
- Prettier
- TypeScript and JavaScript Language Features
- Biome (linter/formatter used in this project)

---

## Development Environment Setup

### Configure Git

```bash
# Set your name and email (if not already set)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Configure pnpm (Optional)

```bash
# Set up pnpm global directory (optional but recommended)
pnpm setup

# Configure pnpm to use Node.js 24
pnpm env use --global 24
```

---

## Clone and Install

### Step 1: Clone the Repository

```bash
# Clone from GitHub
git clone https://github.com/refinedev/refine.git

# Navigate into the directory
cd refine
```

**📁 Repository Size:** ~500 MB (large monorepo with 280+ examples)

### Step 2: Install Dependencies

```bash
# Install all dependencies for all packages
pnpm install
```

**⏱️ Time:** 2-5 minutes (depending on internet speed)

**What happens:**
- pnpm reads `pnpm-workspace.yaml` to find all packages
- Installs dependencies for 40+ packages
- Links packages together in the monorepo
- Sets up git hooks (husky)

**💡 Troubleshooting:**
If installation fails, see [Common Issues](#common-issues)

### Step 3: Verify Installation

```bash
# Check that everything is linked correctly
pnpm list --depth=0

# You should see all @refinedev/* packages listed
```

---

## Understanding the Monorepo

Refine uses a **monorepo** structure managed by Lerna and pnpm workspaces.

### What is a Monorepo?

**🧠 Mental Model:** A monorepo is like a city with many buildings (packages) sharing infrastructure.

**Benefits:**
- ✅ All code in one place
- ✅ Shared dependencies
- ✅ Easy to test changes across packages
- ✅ Consistent tooling

### Repository Structure

```
refine/
├── packages/              # 40 npm packages
│   ├── core/             # @refinedev/core (main package)
│   ├── antd/             # @refinedev/antd (Ant Design integration)
│   ├── mui/              # @refinedev/mui (Material UI integration)
│   ├── react-router/     # @refinedev/react-router
│   ├── nextjs-router/    # @refinedev/nextjs-router
│   ├── supabase/         # @refinedev/supabase
│   └── ...               # 34 more packages
├── examples/             # 280+ example applications
├── documentation/        # Docusaurus documentation site
├── docs/                 # Additional documentation
│   └── learning/         # 👈 YOU ARE HERE
├── cypress/              # E2E test suite
├── scripts/              # Build and utility scripts
└── package.json          # Root package.json
```

[Detailed structure guide →](PROJECT_STRUCTURE.md)

### Key Directories

**`packages/`** - Publishable npm packages
- Each package is independent and can be published
- Core package: `@refinedev/core`
- UI integrations: `@refinedev/{antd,mui,mantine,chakra-ui}`
- Router integrations: `@refinedev/{react-router,nextjs-router,remix-router}`
- Data providers: `@refinedev/{supabase,graphql,simple-rest,...}`

**`examples/`** - Example applications
- 280+ examples showing different use cases
- Great for learning patterns
- Each is a standalone application

**`documentation/`** - Documentation website
- Built with Docusaurus
- Run locally with `cd documentation && pnpm dev`

**`docs/learning/`** - Learning materials
- This guide and others
- Created for developers learning the codebase

---

## Running Your First Example

Let's run a simple example to see Refine in action!

### Step 1: Choose an Example

We'll start with `base-headless` - the simplest example with no UI framework.

```bash
# Navigate to the base-headless example
cd examples/base-headless
```

### Step 2: Start the Development Server

```bash
# Start the dev server
pnpm dev
```

**⏱️ Time:** 10-30 seconds for first build

**Output:**
```
VITE v5.x.x  ready in XXX ms

➜  Local:   http://localhost:5173/
➜  Network: use --host to expose
➜  press h to show help
```

### Step 3: Open in Browser

Open [http://localhost:5173/](http://localhost:5173/)

**🎉 You should see:**
- A basic CRUD application
- A list of blog posts
- Create, Edit, Show buttons

### Step 4: Explore the Code

**Key files to examine:**

1. **`src/App.tsx`** - Main application setup
   ```tsx
   import { Refine } from "@refinedev/core";
   import dataProvider from "@refinedev/simple-rest";
   import routerProvider from "@refinedev/react-router";

   function App() {
     return (
       <Refine
         dataProvider={dataProvider("https://api.fake-rest.refine.dev")}
         routerProvider={routerProvider}
         resources={[
           {
             name: "blog_posts",
             list: "/blog-posts",
             create: "/blog-posts/create",
             edit: "/blog-posts/edit/:id",
             show: "/blog-posts/show/:id",
           },
         ]}
       >
         {/* Routes */}
       </Refine>
     );
   }
   ```

2. **`src/pages/blog-posts/list.tsx`** - List view
   - Uses `useTable()` hook
   - Fetches data automatically
   - Handles pagination

3. **`src/pages/blog-posts/create.tsx`** - Create view
   - Uses `useForm()` hook
   - Handles form submission
   - Shows success notification

**💡 Key Concepts Demonstrated:**
- **Headless architecture** - No UI library, just HTML
- **dataProvider** - Connects to REST API
- **routerProvider** - React Router integration
- **resources** - Defines what you're managing
- **hooks** - `useTable()`, `useForm()`, etc.

### Step 5: Make a Change

Let's modify the list view to add a console log:

**Edit `src/pages/blog-posts/list.tsx`:**
```tsx
export const BlogPostList = () => {
  const { tableQuery, current, setCurrent, pageSize, setPageSize } = useTable();

  // 👇 Add this
  console.log("Table data:", tableQuery.data);

  return (
    // ... rest of component
  );
};
```

**Save the file** and see the console output in your browser DevTools!

**🌉 React Developer Note:** Refine hooks work just like any React hook - they follow the same rules and patterns.

---

## Exploring Examples

The `examples/` directory contains 280+ applications. Here's how to navigate them:

### Beginner-Friendly Examples

**1. Base Examples** - No UI framework complexity

```bash
# Headless (no UI framework)
cd examples/base-headless
pnpm dev

# With Ant Design
cd examples/base-antd
pnpm dev

# With Material UI
cd examples/base-mui
pnpm dev

# With Mantine
cd examples/base-mantine
pnpm dev

# With Chakra UI
cd examples/base-chakra-ui
pnpm dev
```

**2. Authentication Examples**

```bash
# Basic auth with Ant Design
cd examples/auth-antd
pnpm dev

# Auth with Google Login
cd examples/auth-google-login
pnpm dev

# Auth with Auth0
cd examples/auth-auth0
pnpm dev
```

**3. Data Provider Examples**

```bash
# Supabase
cd examples/data-provider-supabase
pnpm dev

# GraphQL
cd examples/data-provider-graphql
pnpm dev

# Strapi
cd examples/data-provider-strapi-v4
pnpm dev
```

### Real-World Applications

**Complete Applications:**

```bash
# FineFood - Complete admin panel for food delivery
cd examples/finefoods-antd
pnpm dev

# FineFood Client - Customer storefront
cd examples/finefoods-client
pnpm dev

# Minimal CRM
cd examples/app-crm-minimal
pnpm dev
```

These show production-ready patterns!

### Finding Examples by Feature

**Naming convention:** `{category}-{framework}-{feature}`

Examples:
- `form-antd-use-modal-form` - Modal form with Ant Design
- `table-mui-cursor-pagination` - Cursor pagination with Material UI
- `customization-theme-mantine` - Theme customization with Mantine

**💡 Pro Tip:** Use `ls examples/ | grep {keyword}` to find relevant examples

```bash
# Find all auth examples
ls examples/ | grep auth

# Find all Ant Design examples
ls examples/ | grep antd

# Find all form examples
ls examples/ | grep form
```

---

## Development Workflow

### Building Packages

**Build a single package:**
```bash
# Build core package only
pnpm build --scope @refinedev/core

# Build a specific package
pnpm build --scope @refinedev/antd
```

**Build all packages:**
```bash
# Build everything
pnpm build:all
```

**⏱️ Time:** 10-30 minutes for full build

### Running Tests

**Test a single package:**
```bash
# Test core package
pnpm test --scope @refinedev/core

# Test with UI
pnpm test:ui --scope @refinedev/core
```

**Test all packages:**
```bash
# Run all tests
pnpm test:all

# With coverage
pnpm test:all:coverage
```

### Linting and Formatting

```bash
# Check code quality
pnpm lint

# Fix auto-fixable issues
pnpm lint:fix

# Format code with Biome
pnpm biome format --write .
```

⚠️ **OUTDATED PATTERN (Nov 2025):** Project uses Biome 1.7.3, but Biome 2.0 is available with type-aware linting.

[Learn more about Biome →](TECH_STACK_RESEARCH.md#biome---v173)

### Watching for Changes

When developing, you might want to watch a package for changes:

```bash
# Watch core package
cd packages/core
pnpm dev
```

Then in another terminal, run an example:

```bash
cd examples/base-headless
pnpm dev
```

Changes to core will rebuild automatically!

---

## Common Issues

### Installation Fails

**Problem:** `pnpm install` fails or hangs

**Solutions:**
```bash
# Clear pnpm cache
pnpm store prune

# Delete node_modules and try again
pnpm nuke
pnpm install

# Or manually:
find . -name 'node_modules' -type d -prune -exec rm -rf '{}' +
pnpm install
```

### Port Already in Use

**Problem:** `Error: Port 5173 is already in use`

**Solutions:**
```bash
# Find what's using the port
lsof -i :5173

# Kill the process
kill -9 <PID>

# Or use a different port
pnpm dev -- --port 3000
```

### Module Not Found

**Problem:** `Cannot find module '@refinedev/core'`

**Solutions:**
```bash
# Re-link packages
pnpm install

# Build dependencies
pnpm build --scope @refinedev/core
```

### TypeScript Errors

**Problem:** TypeScript errors in examples

**Solutions:**
```bash
# Ensure you're using TypeScript 5.8+
tsc --version

# Generate type declarations
cd packages/core
pnpm types
```

⚠️ **Note:** Project uses TypeScript 5.8.3. TypeScript 5.9.3 is the latest stable version as of November 2025.

[TypeScript version details →](TECH_STACK_RESEARCH.md#typescript---v583)

### Node.js Version Mismatch

**Problem:** Errors about Node.js version

**Solutions:**
```bash
# Check Node.js version
node --version

# Switch to Node.js 24 (with nvm)
nvm install 24
nvm use 24

# Or Node.js 22 (also LTS)
nvm install 22
nvm use 22
```

🚨 **CRITICAL:** Do not use Node.js 18 (past EOL as of April 2025)

[Node.js version details →](TECH_STACK_RESEARCH.md#nodejs---18)

### Build Failures

**Problem:** `pnpm build` fails

**Solutions:**
```bash
# Clean build cache
pnpm nuke:builds

# Clean Nx cache
pnpm nuke:nx

# Start fresh
pnpm coffee  # Nukes everything and rebuilds
```

**⚠️ Warning:** `pnpm coffee` takes 20-40 minutes!

---

## Next Steps

Congratulations! You've set up Refine and run your first example. 🎉

### Learning Path

**Choose your next step based on your goal:**

**🎯 I want to understand how Refine works**
→ Read [Architecture Overview](ARCHITECTURE_OVERVIEW.md)

**🎯 I want to build something**
→ Check [How-To Guide](HOW_TO_GUIDE.md)

**🎯 I want to understand the codebase**
→ Read [Project Structure](PROJECT_STRUCTURE.md)

**🎯 I want to learn all the hooks**
→ Study [Hooks Guide](HOOKS_GUIDE.md)

**🎯 I want to see code examples**
→ Follow [Code Tours](CODE_TOURS.md)

### Recommended Reading Order

For complete beginners:
1. ✅ Getting Started (you are here!)
2. [Architecture Overview](ARCHITECTURE_OVERVIEW.md)
3. [Core Concepts](CORE_CONCEPTS.md)
4. [Data Flow Guide](DATA_FLOW_GUIDE.md)
5. [How-To Guide](HOW_TO_GUIDE.md)

### Quick Reference

**🔗 Important Links:**
- [Official Docs](https://refine.dev/docs/)
- [Examples](https://refine.dev/examples/)
- [Discord Community](https://discord.gg/refine)
- [GitHub Repository](https://github.com/refinedev/refine)
- [Blog & Tutorials](https://refine.dev/blog/)

**📚 This Learning Hub:**
- [Central Hub](README.md)
- [FAQ](FAQ.md)
- [All Guides](README.md#complete-documentation-index)

---

## ✅ Quick Check

Test your understanding:

1. **What package manager does Refine use?**
   - Answer: pnpm 9.x

2. **What are the three main parts of the Refine config?**
   - Answer: `dataProvider`, `routerProvider`, `resources` (and more!)

3. **Where are example applications located?**
   - Answer: `examples/` directory

4. **What hook would you use for a table/list view?**
   - Answer: `useTable()`

5. **What is the core package name?**
   - Answer: `@refinedev/core`

---

## 🎯 Remember This

**💡 The Refine Setup Formula:**

```
Refine App = <Refine /> + Providers + Resources + Your UI
```

**🌉 React Developer Bridge:**

Refine is just React! All the hooks, components, and patterns you know still apply. Refine adds structure and utilities on top.

---

## Getting Help

**Stuck?**
- 💬 [Discord](https://discord.gg/refine) - Ask the community
- 📖 [FAQ](FAQ.md) - Common questions
- 🐛 [GitHub Issues](https://github.com/refinedev/refine/issues) - Report bugs
- 🔍 [Debugging Guide](DEBUGGING_GUIDE.md) - Debug tips

**Contributing:**
- 🤝 [First Contributions](FIRST_CONTRIBUTIONS.md) - Get started
- 📝 [Development Workflow](DEVELOPMENT_WORKFLOW.md) - Full guide

---

<div align="center">

**You're Ready!** 🚀

[Architecture Overview →](ARCHITECTURE_OVERVIEW.md)

</div>

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
**Next Review:** After major version updates
