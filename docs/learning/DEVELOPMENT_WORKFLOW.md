# Development Workflow

**📅 Documented:** November 18, 2025
**👥 Audience:** Contributors
**⏱️ Estimated Reading:** 20-25 minutes

---

## Getting Started

### Clone & Install

```bash
git clone https://github.com/refinedev/refine.git
cd refine
pnpm install
```

---

## Development Commands

### Build

```bash
# Build single package
pnpm build --scope @refinedev/core

# Build all
pnpm build:all
```

### Test

```bash
# Run tests
pnpm test

# With coverage
pnpm test:coverage

# All packages
pnpm test:all
```

### Lint

```bash
pnpm lint
pnpm lint:fix
```

---

## Working on a Package

```bash
# 1. Navigate to package
cd packages/core

# 2. Watch mode
pnpm dev

# 3. In another terminal, run example
cd examples/base-headless
pnpm dev

# Changes rebuild automatically!
```

---

## Creating a PR

1. Fork the repository
2. Create feature branch: `git checkout -b feat/my-feature`
3. Make changes
4. Run tests: `pnpm test`
5. Commit: `git commit -m "feat: add feature"`
6. Push: `git push origin feat/my-feature`
7. Open PR on GitHub

---

## Commit Conventions

Use conventional commits:

```
feat: add new feature
fix: fix bug
docs: update documentation
chore: update dependencies
refactor: refactor code
test: add tests
```

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
