# Technology Stack Research (November 2025)

**Research conducted:** November 18, 2025
**Researcher:** Claude AI
**Purpose:** Document current state of all technologies used in the Refine project

---

## Executive Summary

This document provides a comprehensive analysis of all technologies used in the Refine project, comparing the versions currently in use against the latest stable releases as of November 2025. This research is critical because AI knowledge cutoffs (January 2025) mean that significant updates may have occurred in the past 10 months.

**Overall Project Health:** 🟡 **Moderate - Several major versions behind**

The project uses modern, well-maintained technologies but is behind on several major version updates that offer significant improvements in performance, features, and developer experience.

---

## Core Technologies

### React - v19.1.0

**Current Status (Nov 2025):**
- Latest stable: **v19.2.0** (October 2025)
- Project uses: v19.1.0 (also v17.0.2 in documentation)
- Status: ⚠️ **Slightly outdated** - One minor version behind

**Important Updates Since Jan 2025:**

**React 19.x (Stable since April 2025):**
- React Compiler for automatic optimization
- Actions for form handling
- use() API for reading promises/context
- Server Components (stable)
- Async Request APIs

**React 19.2 (October 2025):**
- `<Activity>` component - New API to hide/restore UI and internal state
- `useEffectEvent` Hook - Extract non-reactive logic from Effects
- `cacheSignal` for RSCs - Know when cache() lifetime is over
- Performance tracks in browser DevTools
- `eslint-plugin-react-hooks` v6 with flat config
- Updated useId prefix from `:r:` to `_r_` for View Transitions compatibility

**What This Means for Learning:**
- ✅ The project uses modern React 19.x patterns
- ⚠️ React 19.2 features (Oct 2025) like `<Activity>` and `useEffectEvent` are not yet available
- The documentation site uses React 17, which is significantly outdated for new features

**Official Resources:**
- Docs: https://react.dev/
- Blog: https://react.dev/blog
- React 19.2 Announcement: https://react.dev/blog/2025/10/01/react-19-2
- Migration Guide: https://react.dev/blog/2024/04/25/react-19

**Recommendation:**
- Minor update to 19.2 recommended to get latest features
- Strongly consider upgrading documentation site from React 17 to 19.x

---

### TypeScript - v5.8.3

**Current Status (Nov 2025):**
- Latest stable: **v5.9.3** (October 2025)
- Project uses: v5.8.3
- Status: ⚠️ **One minor version behind**

**Important Updates Since Jan 2025:**

**TypeScript 5.8 (Currently used):**
- Better type inference
- Performance improvements
- Compiler optimizations

**TypeScript 5.9.x (August - October 2025):**
- Deferred imports
- Improved default project setup via scaffolding flag
- More stable module resolution mode for Node.js v20
- Expandable hover previews
- Enhanced developer experience

**Looking Ahead - TypeScript 6.0 & 7.0:**
- TypeScript 6.0 is in development (6.0.0-dev published Nov 16, 2025)
- TypeScript 6.0 will act as a transition point for TypeScript 7.0
- TypeScript 7.0 will be a native Go port with 10x faster performance
- These are NOT yet stable for production use

**What This Means for Learning:**
- ✅ The project uses modern TypeScript 5.8 - patterns are current
- ⚠️ Missing some DX improvements from 5.9
- The TypeScript ecosystem is preparing for major changes with 6.0/7.0

**Official Resources:**
- Docs: https://www.typescriptlang.org/docs/
- 5.9 Release: https://devblogs.microsoft.com/typescript/announcing-typescript-5-9/
- Releases: https://github.com/microsoft/typescript/releases

**Recommendation:**
- Update to 5.9.3 for latest bug fixes and DX improvements
- Monitor TypeScript 6.0 development for breaking changes

---

### Node.js - >= 18

**Current Status (Nov 2025):**
- Latest LTS: **v24.11.0 "Krypton"** (October 2025, supported until April 2028)
- Also current: **v22.x "Jod"** (Active LTS until October 2027)
- Project requires: >= 18
- Status: ⚠️ **Minimum requirement is one major version behind current LTS**

**Important Updates Since Jan 2025:**

**Node.js 24.x "Krypton" (Current LTS - Oct 2025):**
- Entered Long Term Support on October 28, 2025
- Will receive updates through April 2028
- Recommended for all new production applications
- Performance improvements
- Enhanced security features

**Node.js 22.x "Jod" (Active LTS):**
- Still in "Active LTS" mode through October 2025
- Then moves to "Maintenance" mode until April 2027
- Stable and production-ready

**Node.js 18.x (Project minimum):**
- Entered maintenance mode in October 2024
- End of Life: April 2025 (already passed!)
- Should not be used for new projects

**What This Means for Learning:**
- ⚠️ Node.js 18 is past EOL - security risk
- Developers should use Node.js 24.x LTS for development
- Project should update minimum requirement to >= 22 or >= 24

**Official Resources:**
- Releases: https://nodejs.org/en/about/previous-releases
- LTS Schedule: https://github.com/nodejs/release#release-schedule
- Node.js 24 Release: https://nodejs.org/en/blog/release/v24.11.0

**Recommendation:**
- 🚨 **CRITICAL:** Update minimum Node.js requirement to >= 22
- Recommended: >= 24 for latest LTS
- Test all packages with Node.js 24.x

---

### pnpm - v9.4.0

**Current Status (Nov 2025):**
- Project uses: v9.4.0 (via packageManager field)
- Project requires: >= 9
- Status: ✅ **Modern version** - Actively maintained

**Important Updates Since Jan 2025:**
- Still actively developed with regular releases
- Network performance monitoring added
- Improved support for monorepos
- Enhanced workspace features
- Better dependency resolution

**What This Means for Learning:**
- ✅ pnpm 9.x is the current major version
- pnpm is the recommended choice for monorepos in 2025
- Excellent for disk space optimization and build performance

**Official Resources:**
- Docs: https://pnpm.io/
- Releases: https://github.com/pnpm/pnpm/releases
- Installation: https://pnpm.io/installation

**Recommendation:**
- ✅ Current setup is good
- Stay on pnpm 9.x line, apply patch updates regularly

---

## Build & Tooling

### Lerna - v8.1.2

**Current Status (Nov 2025):**
- Latest stable: **v9.0.1** (November 2025)
- Project uses: v8.1.2
- Status: 🚨 **One major version behind**

**Important Updates Since Jan 2025:**

**Lerna 9.0.x (November 2025):**
- 🚨 **Breaking:** Node v18 support dropped (EOL)
- Supported Node versions: ^20.19.0 || ^22.12.0 || >=24.0.0
- 🚨 **Breaking:** `@lerna/legacy-package-management` formally removed after 2 years
- Legacy commands removed: `lerna add`, `lerna bootstrap`, `lerna link`
- Must use package manager workspaces (npm/yarn/pnpm) instead
- Run `lerna repair` after updating to migrate lerna.json

**Modern Lerna Features (2025):**
- Local & remote caching support (powered by Nx)
- Task pipelines
- Improved terminal output
- Prettier support
- NPM/Yarn/PNPM workspaces integration
- Built-in versioning and publishing workflow

**Maintenance Status:**
- Maintained by Nrwl (the Nx team) since May 2022
- Actively developed and supported
- Co-exists in the Nx ecosystem

**What This Means for Learning:**
- ⚠️ This project needs to migrate to Lerna 9 soon
- Must ensure no legacy commands are being used
- The modern pattern is: pnpm workspaces + Lerna for versioning/publishing

**Official Resources:**
- Docs: https://lerna.js.org/
- Releases: https://github.com/lerna/lerna/releases
- Migration Guide: https://lerna.js.org/docs/lerna-and-nx-version-matrix
- Upgrade Guide: https://lerna.js.org/upgrade

**Recommendation:**
- 🚨 **HIGH PRIORITY:** Upgrade to Lerna 9.x
- Verify no legacy commands in use
- Run `lerna repair` after upgrade
- Update CI/CD for Node.js 20+ requirement

---

### Nx - v18.2.2

**Current Status (Nov 2025):**
- Latest stable: **v22.x** (November 2025)
- Project uses: v18.2.2
- Status: 🚨 **Four major versions behind**

**Important Updates Since Jan 2025:**

**Nx 22.x (November 2025):**
- .NET and Maven support (polyglot enterprises)
- pnpm catalog integration for easier dependency upgrades
- Release workflow improvements
- Database-backed caching (now default for all workspaces)
- CreateNodes V2 (V1 removed)
- Legacy TypeScript plugin behavior changed

**Breaking Changes in Nx 22:**
- All workspaces use database-backed caching (auto-migration)
- CreateNodes V1 removed - plugin authors must migrate to V2
- `useLegacyTypescriptPlugin` now defaults to false

**What This Means for Learning:**
- 🚨 Project is significantly behind on Nx updates
- Major performance improvements available in newer versions
- Breaking changes will require careful migration

**Official Resources:**
- Docs: https://nx.dev/
- Nx 22 Release: https://nx.dev/blog/nx-22-release
- Release Schedule: https://nx.dev/docs/reference/releases
- Migration Guide: https://nx.dev/recipes/adopting-nx/migration-guide

**Recommendation:**
- 🚨 **HIGH PRIORITY:** Plan migration to Nx 22
- Test thoroughly due to breaking changes
- Leverage migration scripts: `nx migrate latest`

---

### Biome - v1.7.3

**Current Status (Nov 2025):**
- Latest stable: **v2.0** (June 2025)
- Project uses: v1.7.3
- Status: 🚨 **One major version behind**

**Important Updates Since Jan 2025:**

**Biome 2.0 (June 2025):**
- Plugin system introduced
- Type-aware linting (catches bugs without full TS compiler)
- 800,000+ weekly downloads
- Won JSNation's Productivity Booster Award 2024
- Enhanced Rust-based performance

**What is Biome:**
- All-in-one toolchain replacing ESLint + Prettier
- 97% Prettier compatibility
- 373+ linting rules from ESLint, TypeScript ESLint, etc.
- Formatter for JS/TS/JSX/JSON/CSS/GraphQL
- Written in Rust for speed

**What This Means for Learning:**
- ⚠️ Missing type-aware linting from 2.0
- ⚠️ Missing plugin system from 2.0
- Current version is functional but outdated

**Official Resources:**
- Docs: https://biomejs.dev/
- Linter: https://biomejs.dev/linter/
- Migration Guide: https://blog.appsignal.com/2025/05/07/migrating-a-javascript-project-from-prettier-and-eslint-to-biomejs.html

**Recommendation:**
- Update to Biome 2.0 for type-aware linting
- Review breaking changes before upgrading

---

### tsup - v6.7.0

**Current Status (Nov 2025):**
- Project uses: v6.7.0
- Status: 🚨 **NO LONGER ACTIVELY MAINTAINED**

**Important Updates:**
- ⚠️ tsup is no longer actively maintained
- Recommended alternative: **tsdown**
- Last updates were in early 2025

**What is tsup:**
- Zero-configuration TypeScript bundler
- Powered by esbuild
- Fast compilation and optimization
- Multiple module format outputs

**What This Means for Learning:**
- ⚠️ The project uses a tool that's deprecated
- Still functional but won't receive updates
- Security and compatibility risks over time

**Official Resources:**
- GitHub: https://github.com/egoist/tsup
- Docs: https://tsup.egoist.dev/
- Recommended alternative: tsdown

**Recommendation:**
- 🚨 **MEDIUM PRIORITY:** Plan migration to tsdown
- Research tsdown compatibility
- Test build process thoroughly during migration

---

## Testing & Quality

### Vitest - v2.1.8

**Current Status (Nov 2025):**
- Latest stable: **v4.0.8** (October 2025)
- Project uses: v2.1.8
- Status: 🚨 **Two major versions behind**

**Important Updates Since Jan 2025:**

**Vitest 4.0 (October 2025):**
- Browser Mode promoted from experimental to **stable**
- Visual Regression testing in Browser Mode
- Playwright Traces integration for detailed debugging
- Major performance improvements
- Breaking changes - review migration guide

**Vite 7.0 Support:**
- Supported from Vitest 3.2 onwards
- Enhanced compatibility with latest build tools

**What This Means for Learning:**
- 🚨 Missing stable Browser Mode (major testing feature)
- 🚨 Missing Visual Regression testing
- 🚨 Missing performance improvements from v3 and v4
- Will need migration guide for upgrade

**Official Resources:**
- Docs: https://vitest.dev/
- Vitest 4 Announcement: https://vitest.dev/blog/vitest-4
- Migration Guide: https://vitest.dev/guide/migration.html

**Recommendation:**
- 🚨 **HIGH PRIORITY:** Upgrade to Vitest 4.0
- Review breaking changes carefully
- Leverage stable Browser Mode for better testing

---

### Cypress - v13.6.3

**Current Status (Nov 2025):**
- Latest stable: **v15.6.0** (November 2025)
- Project uses: v13.6.3
- Status: 🚨 **Two major versions behind**

**Important Updates Since Jan 2025:**

**Cypress 14.x:**
- New features and performance improvements
- Enhanced debugging capabilities

**Cypress 15.x (Latest - Nov 2025):**
- v15.6.0 released November 4, 2025
- v15.5.0 released October 18, 2025
- v15.4.0 released October 7, 2025
- Significant improvements in stability and features

**What This Means for Learning:**
- 🚨 Missing two major versions of improvements
- Potential security and compatibility issues
- Missing latest debugging and performance features

**Official Resources:**
- Docs: https://docs.cypress.io/
- Changelog: https://docs.cypress.io/app/references/changelog
- Releases: https://github.com/cypress-io/cypress/releases

**Recommendation:**
- 🚨 **HIGH PRIORITY:** Upgrade to Cypress 15.x
- Review changelog for breaking changes
- Update test selectors if needed

---

## Core Dependencies

### TanStack Query (React Query) - v5.81.5

**Current Status (Nov 2025):**
- Latest stable: **v5.90.7** (November 2025)
- Project uses: v5.81.5
- Status: ⚠️ **Minor version behind** (9 patch versions)

**Important Updates Since Jan 2025:**

**TanStack Query v5 (Major Features):**
- ✅ Full Suspense support (no longer experimental)
- Renamed: `isLoading` → `isPending`
- Renamed: `useErrorBoundary` → `throwOnError`
- Renamed: `Hydrate` → `HydrationBoundary`
- New hooks: `useSuspenseQuery`, `useSuspenseInfiniteQuery`, `useSuspenseQueries`
- New hook: `useMutationState` for tracking all mutations
- Infinite queries require explicit `initialPageParam`
- New `maxPages` option for infinite queries
- Server-side retry defaults to 0 instead of 3
- Framework-agnostic devtools with UI revamp
- Cache inline editing and light mode in devtools
- ~20% smaller than v4

**What This Means for Learning:**
- ✅ The project uses modern TanStack Query v5
- ✅ All patterns shown will be current
- ⚠️ Minor patch updates available (bug fixes)

**Official Resources:**
- Docs: https://tanstack.com/query/latest
- Migration to v5: https://tanstack.com/query/latest/docs/framework/react/guides/migrating-to-v5
- Releases: https://github.com/TanStack/query/releases

**Recommendation:**
- Low priority: Update to 5.90.7 for bug fixes
- Current version is functional and modern

---

## Documentation

### Docusaurus - v2.4.0

**Current Status (Nov 2025):**
- Latest stable: **v3.9** (October 2025)
- Project uses: v2.4.0
- Status: 🚨 **One major version behind**

**Important Updates Since Jan 2025:**

**Docusaurus 3.x Series:**
- Modernized runtime environment
- React 18+ required
- Node 18 minimum (v3.9 drops Node 18 support)

**Docusaurus 3.9 (October 2025):**
- Full support for Algolia DocSearch v4
- **Ask AI** feature built into documentation
- Advanced i18n configurations
- Per-locale `baseUrl` and `url` overrides
- Multi-domain and deeply localized deployments
- 🚨 Drops Node 18 support

**What This Means for Learning:**
- 🚨 Documentation site is one major version behind
- 🚨 Missing AI-powered search capabilities
- 🚨 Missing modern i18n features
- Potential security and compatibility issues

**Official Resources:**
- Docs: https://docusaurus.io/
- Changelog: https://docusaurus.io/changelog
- Releases: https://github.com/facebook/docusaurus/releases
- Docusaurus 3.9 Info: https://www.infoq.com/news/2025/10/docusaurus-3-9-ai-search/

**Recommendation:**
- 🚨 **HIGH PRIORITY:** Upgrade to Docusaurus 3.9
- Update Node.js requirement (20+)
- Leverage new AI search features
- May need React upgrade in docs

---

## Framework Patterns & Best Practices (Nov 2025)

### React Router vs Next.js Router

**Current Industry Standard (2025):**

**For Next.js Projects:**
- Use **Next.js App Router** (file-based routing)
- DO NOT use react-router-dom in Next.js apps
- App Router is the definitive standard as of 2025

**App Router Features:**
- Server Components (default)
- File-based routing
- Nested layouts
- Streaming responses
- Better caching
- `layout.js`, `page.js`, `loading.js`, `error.js`

**2025 Best Practices:**
1. **Server Components First** - Push 'use client' as far down as possible
2. **Avoid 'use client' at root** - Removes many RSC benefits
3. **File-based routing** - Files in `/app` become routes automatically
4. **TypeScript mandatory** - Type safety is paramount in 2025

**For React-Only Projects:**
- Use **React Router v7** or **TanStack Router**
- React Router v7 is the latest as of 2025

**What This Means for Refine:**
- ✅ Refine provides routing integrations for multiple routers
- Packages: `@refinedev/nextjs-router`, `@refinedev/react-router`, `@refinedev/remix-router`
- This flexibility is a core Refine feature

**Official Resources:**
- Next.js Routing: https://nextjs.org/docs/app/building-your-application/routing
- React Router: https://reactrouter.com/
- TanStack Router: https://tanstack.com/router

---

## UI Framework Integrations

The Refine project provides integrations for multiple UI frameworks. Here's their status as of November 2025:

### Ant Design
- ✅ Active and maintained
- Latest: v5.x
- Status: Current

### Material UI (MUI)
- ✅ Active and maintained
- Latest: v6.x (as of late 2024/early 2025)
- Status: Current

### Mantine
- ✅ Active and maintained
- Latest: v7.x
- Status: Current

### Chakra UI
- ✅ Active and maintained
- Latest: v3.x (major rewrite in 2024)
- Status: Current

### TailwindCSS (Headless)
- ✅ Active and maintained
- Latest: v3.4.x
- Status: Current

**What This Means for Learning:**
- ✅ All UI framework integrations are with actively maintained libraries
- Check each integration's package.json for specific versions
- Refine's headless architecture allows using any UI framework

---

## Data Providers

Refine integrates with multiple backend services. Status as of November 2025:

### Supabase
- ✅ Active and maintained
- Supabase continues strong growth in 2025

### Appwrite
- ✅ Active and maintained
- v1.6+ as of late 2024/2025

### GraphQL
- ✅ Active ecosystem
- GraphQL is industry standard

### Hasura
- ✅ Active and maintained
- GraphQL engine for PostgreSQL

### Strapi
- ✅ Active and maintained
- v4 is current (v5 in beta as of late 2024)

### Airtable
- ✅ Active
- API stable and maintained

### NestJS
- ✅ Very active
- v10+ as of 2024/2025

### Medusa
- ✅ Active and growing
- v2.0 released in 2024

**What This Means for Learning:**
- ✅ All data provider integrations are with active projects
- Developers can confidently learn any of these integrations
- The ecosystem is healthy and growing

---

## Key Packages Status Summary

| Package | Current Version | Latest Version | Status | Priority |
|---------|----------------|----------------|--------|----------|
| React | 19.1.0 | 19.2.0 | ⚠️ Minor behind | Low |
| TypeScript | 5.8.3 | 5.9.3 | ⚠️ Minor behind | Medium |
| Node.js (min) | >= 18 | >= 24 LTS | 🚨 Major behind | **CRITICAL** |
| pnpm | 9.4.0 | 9.x | ✅ Current | None |
| Lerna | 8.1.2 | 9.0.1 | 🚨 Major behind | **HIGH** |
| Nx | 18.2.2 | 22.x | 🚨 4 majors behind | **HIGH** |
| Biome | 1.7.3 | 2.0 | 🚨 Major behind | Medium |
| tsup | 6.7.0 | Deprecated | 🚨 Unmaintained | Medium |
| Vitest | 2.1.8 | 4.0.8 | 🚨 2 majors behind | **HIGH** |
| Cypress | 13.6.3 | 15.6.0 | 🚨 2 majors behind | **HIGH** |
| TanStack Query | 5.81.5 | 5.90.7 | ⚠️ Patch behind | Low |
| Docusaurus | 2.4.0 | 3.9 | 🚨 Major behind | **HIGH** |

---

## Recommendations & Action Items

### Critical Priority (Security & Maintenance)

1. **Update Node.js Requirement**
   - 🚨 Node.js 18 is past EOL (April 2025)
   - Update `engines.node` to `>= 22` or `>= 24`
   - Test all packages with Node.js 24.x LTS
   - Update CI/CD pipelines

### High Priority (Major Version Updates)

2. **Upgrade Lerna to 9.0.1**
   - Breaking changes: Node 18 dropped, legacy commands removed
   - Run `lerna repair` after upgrade
   - Ensure using pnpm workspaces, not legacy bootstrap

3. **Upgrade Nx to 22.x**
   - 4 major versions behind
   - Significant performance and feature improvements
   - Use `nx migrate latest` for automated migration
   - Test thoroughly due to breaking changes

4. **Upgrade Vitest to 4.0**
   - Stable Browser Mode available
   - Visual regression testing
   - Performance improvements
   - Review migration guide

5. **Upgrade Cypress to 15.6.0**
   - 2 major versions behind
   - Latest features and stability
   - Security improvements

6. **Upgrade Docusaurus to 3.9**
   - AI-powered search capabilities
   - Modern i18n features
   - Better React 18+ support
   - May require Node 20+

### Medium Priority (Minor Updates & Improvements)

7. **Update TypeScript to 5.9.3**
   - Latest bug fixes
   - Improved developer experience
   - Prepare for TypeScript 6.0/7.0

8. **Update Biome to 2.0**
   - Type-aware linting
   - Plugin system
   - Better error detection

9. **Migrate from tsup to tsdown**
   - tsup is no longer maintained
   - Prevents future compatibility issues

### Low Priority (Patch Updates)

10. **Update React to 19.2**
    - Latest features (Activity, useEffectEvent)
    - Performance tracks in DevTools
    - Minor improvements

11. **Update TanStack Query to 5.90.7**
    - Bug fixes and minor improvements
    - Already on modern v5

### Long-term Considerations

12. **Monitor TypeScript 6.0/7.0**
    - TypeScript 6.0 is in development
    - Will be transition point for TypeScript 7.0
    - TypeScript 7.0 will be 10x faster (Go port)
    - Plan for migration when stable

13. **Documentation Site React Upgrade**
    - Currently using React 17.0.2
    - Should upgrade to React 19.x
    - Aligns with main project

---

## Learning Path Implications

### For New Developers (November 2025)

**What to study:**
1. ✅ **React 19.x** - Current version, patterns are modern
2. ✅ **TypeScript 5.8+** - Current practices
3. ✅ **TanStack Query v5** - Industry standard for data fetching
4. ✅ **Monorepo patterns** - Lerna + pnpm workspaces
5. ⚠️ **Be aware of version gaps** - Several tools are behind

**What to be cautious about:**
1. ⚠️ Some patterns may reference outdated tool versions
2. ⚠️ Check official docs for the specific version in use
3. ⚠️ Be aware that the project is planning major updates

**Best practices:**
1. Always reference the version-specific documentation
2. Check this TECH_STACK_RESEARCH.md for currency
3. When in doubt, check the package.json for exact versions
4. Follow official migration guides when versions update

---

## Conclusion

The Refine project uses a solid, modern technology stack built on React 19.x, TypeScript 5.8, and industry-standard tools. However, several key dependencies are behind on major versions, particularly:

- **Critical:** Node.js minimum requirement (18 is EOL)
- **High Priority:** Lerna, Nx, Vitest, Cypress, Docusaurus

These updates are important for:
- Security (Node.js EOL)
- Performance (Nx, Vitest improvements)
- Features (Vitest Browser Mode, Docusaurus AI search)
- Developer Experience (Biome 2.0, TypeScript 5.9)

The project's architecture remains sound, and the chosen technologies are all actively maintained and well-regarded in the industry as of November 2025.

---

**Next Steps:**
1. Review this research with the team
2. Prioritize updates based on the recommendations
3. Create migration tickets for high-priority items
4. Update learning materials to reflect current state
5. Plan testing strategy for major version upgrades

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
**Next Review:** After major version updates or in 3 months (February 2026)
