# Frequently Asked Questions

**📅 Documented:** November 18, 2025
**👥 Audience:** All developers

---

## General Questions

### What is Refine?

Refine is an open-source React meta-framework for building CRUD-heavy enterprise applications (admin panels, dashboards, internal tools, B2B apps).

---

### Is Refine free?

Yes! Refine is open-source under MIT license. Free for commercial use.

---

### Do I need to know React?

Yes. Refine is built on React. You should understand:
- React components
- React hooks
- JSX
- Basic TypeScript

---

## Technical Questions

### Can I use Refine with Next.js?

Yes! Refine has official Next.js integration:
```bash
npm install @refinedev/nextjs-router
```

✅ **CURRENT (Nov 2025):** Next.js App Router is supported and recommended.

---

### Can I use Refine without a UI framework?

Yes! Refine core is headless. Use it with:
- Plain HTML/CSS
- TailwindCSS
- Your own components
- Any UI library

---

### Which UI framework should I choose?

Depends on your needs:
- **Ant Design** - Enterprise, comprehensive
- **Material UI** - Material Design, popular
- **Mantine** - Modern, great DX
- **Chakra UI** - Accessible
- **Headless** - Full control

---

### Can I use Refine with GraphQL?

Yes!
```bash
npm install @refinedev/graphql graphql-request
```

---

### Does Refine support real-time?

Yes! With live providers:
- Supabase (built-in)
- Custom WebSocket
- Any real-time backend

---

### Can I use multiple data providers?

Yes!
```tsx
<Refine
  dataProvider={{
    default: restProvider,
    graphql: graphqlProvider,
  }}
/>
```

---

## Comparison Questions

### Refine vs React Admin?

| Aspect | Refine | React Admin |
|--------|--------|-------------|
| Philosophy | Headless | Material UI focused |
| TypeScript | Native | Added later |
| Bundle Size | Smaller | Larger |
| Flexibility | Higher | Lower |

Both are excellent! Choose based on needs.

---

### Refine vs Next.js?

They're complementary! Use them together:
- Next.js: Framework (routing, SSR)
- Refine: CRUD layer on top

---

### Refine vs Retool/Appsmith?

| Aspect | Refine | Low-Code Tools |
|--------|--------|----------------|
| Type | Code framework | Visual builder |
| Flexibility | Very high | Limited |
| Learning | React knowledge | Quick start |
| Control | Full | Limited |

Use Refine if you want code control.

---

## Troubleshooting

### Data not loading?

Check:
1. Is resource defined in `<Refine>`?
2. Is data provider configured?
3. Check network tab for errors
4. Check console for errors

See [Debugging Guide](DEBUGGING_GUIDE.md)

---

### Form not submitting?

Check:
1. Is `onFinish` called?
2. Check console for errors
3. Check network tab for API call
4. Verify data provider `create`/`update` methods

---

### Authentication not working?

Check:
1. Is authProvider configured?
2. Does `check()` return correct shape?
3. Are routes protected with `<Authenticated>`?
4. Check localStorage/cookies for token

---

## Contribution Questions

### How can I contribute?

See [First Contributions](FIRST_CONTRIBUTIONS.md)

**Easy starts:**
- Documentation improvements
- Example additions
- Bug reports
- Translations

---

### Where do I report bugs?

[GitHub Issues](https://github.com/refinedev/refine/issues)

Include:
- Refine version
- Steps to reproduce
- Expected vs actual behavior
- Code example if possible

---

## Learning Questions

### Where do I start?

1. [Getting Started](GETTING_STARTED.md)
2. [Architecture Overview](ARCHITECTURE_OVERVIEW.md)
3. [Core Concepts](CORE_CONCEPTS.md)
4. Build something!

---

### Are there tutorials?

Yes!
- [Official Docs](https://refine.dev/docs/)
- [Blog](https://refine.dev/blog/)
- [Examples](../../examples/)
- [Code Tours](CODE_TOURS.md)

---

### Is there a community?

Yes!
- [Discord](https://discord.gg/refine) - Most active
- [Twitter](https://twitter.com/refine_dev)
- [GitHub Discussions](https://github.com/refinedev/refine/discussions)

---

## Still have questions?

**Ask on:**
- [Discord](https://discord.gg/refine) - Fastest
- [GitHub Discussions](https://github.com/refinedev/refine/discussions) - Searchable
- [Stack Overflow](https://stackoverflow.com/questions/tagged/refine) - Public

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
