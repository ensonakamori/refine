# Testing Guide

**📅 Documented:** November 18, 2025
**👥 Audience:** Quality-focused developers
**⏱️ Estimated Reading:** 25-30 minutes

---

## Testing Stack

- **Vitest** - Unit test runner
- **@testing-library/react** - React testing
- **Cypress** - E2E testing

---

## Unit Testing

### Testing Hooks

```tsx
import { renderHook, waitFor } from "@testing-library/react";
import { useList } from "@refinedev/core";
import { TestWrapper } from "./test-wrapper";

describe("useList", () => {
  it("fetches list data", async () => {
    const { result } = renderHook(
      () => useList({ resource: "products" }),
      { wrapper: TestWrapper }
    );

    await waitFor(() => {
      expect(result.current.data).toBeDefined();
    });
  });
});
```

### Test Wrapper

```tsx
// test-wrapper.tsx
import { Refine } from "@refinedev/core";

export const TestWrapper = ({ children }) => {
  return (
    <Refine
      dataProvider={mockDataProvider}
      resources={[{ name: "products" }]}
    >
      {children}
    </Refine>
  );
};

const mockDataProvider = {
  getList: () => Promise.resolve({ data: [], total: 0 }),
  // ... other methods
};
```

---

## Component Testing

```tsx
import { render, screen } from "@testing-library/react";
import { ProductList } from "./ProductList";

test("renders product list", () => {
  render(<ProductList />, { wrapper: TestWrapper });

  expect(screen.getByText("Products")).toBeInTheDocument();
});
```

---

## E2E Testing

```javascript
// cypress/e2e/products.cy.ts
describe("Products CRUD", () => {
  it("creates a product", () => {
    cy.visit("/products/create");
    cy.get('input[name="name"]').type("Test Product");
    cy.get('input[name="price"]').type("99.99");
    cy.get('button[type="submit"]').click();
    cy.url().should("include", "/products");
  });
});
```

---

## Mocking

### Mock Data Provider

```tsx
const mockDataProvider = {
  getList: jest.fn(() => Promise.resolve({
    data: [
      { id: 1, name: "Product 1", price: 100 },
    ],
    total: 1,
  })),
  getOne: jest.fn((params) => Promise.resolve({
    data: { id: params.id, name: "Product", price: 100 },
  })),
  create: jest.fn((params) => Promise.resolve({
    data: { id: 1, ...params.variables },
  })),
};
```

---

## Best Practices

- Test behavior, not implementation
- Use meaningful test descriptions
- Keep tests isolated
- Mock external dependencies
- Test error cases

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
