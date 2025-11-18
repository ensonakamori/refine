# UI Integrations Guide

**📅 Documented:** November 18, 2025
**🔬 Tech Stack:** [View Research](TECH_STACK_RESEARCH.md)
**👥 Audience:** Developers choosing UI framework
**⏱️ Estimated Reading:** 30-35 minutes

---

## Overview

Refine supports multiple UI frameworks through integration packages:

- **Ant Design** (@refinedev/antd)
- **Material UI** (@refinedev/mui)
- **Mantine** (@refinedev/mantine)
- **Chakra UI** (@refinedev/chakra-ui)
- **Headless** (bring your own UI)

**🧠 Mental Model:** UI packages are pre-built components that work with Refine hooks.

---

## Ant Design Integration

### Installation

```bash
npm install @refinedev/antd antd
```

### Setup

```tsx
import { Refine } from "@refinedev/core";
import { ThemedLayoutV2, RefineThemes } from "@refinedev/antd";
import { ConfigProvider } from "antd";

import "@refinedev/antd/dist/reset.css";

function App() {
  return (
    <ConfigProvider theme={RefineThemes.Blue}>
      <Refine>
        <ThemedLayoutV2>
          <Routes>
            {/* routes */}
          </Routes>
        </ThemedLayoutV2>
      </Refine>
    </ConfigProvider>
  );
}
```

### Components

**Layout:**
```tsx
import { ThemedLayoutV2, ThemedSiderV2, ThemedHeaderV2, ThemedTitleV2 } from "@refinedev/antd";

<ThemedLayoutV2
  Header={() => <ThemedHeaderV2 />}
  Sider={() => <ThemedSiderV2 />}
  Title={() => <ThemedTitleV2 />}
>
  <Outlet />
</ThemedLayoutV2>
```

**CRUD Pages:**
```tsx
import { List, Create, Edit, Show } from "@refinedev/antd";

function ProductList() {
  return (
    <List>
      {/* Your table */}
    </List>
  );
}

function ProductCreate() {
  return (
    <Create>
      {/* Your form */}
    </Create>
  );
}
```

**Form:**
```tsx
import { useForm, Create } from "@refinedev/antd";
import { Form, Input, InputNumber } from "antd";

function ProductCreate() {
  const { formProps, saveButtonProps } = useForm();

  return (
    <Create saveButtonProps={saveButtonProps}>
      <Form {...formProps} layout="vertical">
        <Form.Item label="Name" name="name" rules={[{ required: true }]}>
          <Input />
        </Form.Item>
        <Form.Item label="Price" name="price">
          <InputNumber />
        </Form.Item>
      </Form>
    </Create>
  );
}
```

**Table:**
```tsx
import { List, useTable } from "@refinedev/antd";
import { Table } from "antd";

function ProductList() {
  const { tableProps } = useTable();

  return (
    <List>
      <Table {...tableProps} rowKey="id">
        <Table.Column dataIndex="id" title="ID" />
        <Table.Column dataIndex="name" title="Name" />
        <Table.Column dataIndex="price" title="Price" />
      </Table>
    </List>
  );
}
```

**Buttons:**
```tsx
import {
  DeleteButton,
  EditButton,
  ShowButton,
  CreateButton,
  RefreshButton,
  ExportButton,
} from "@refinedev/antd";

<EditButton recordItemId={record.id} />
<DeleteButton recordItemId={record.id} />
<ShowButton recordItemId={record.id} />
```

---

## Material UI Integration

### Installation

```bash
npm install @refinedev/mui @mui/material @mui/x-data-grid @emotion/react @emotion/styled
```

### Setup

```tsx
import { Refine } from "@refinedev/core";
import { ThemedLayoutV2, RefineSnackbarProvider } from "@refinedev/mui";
import { CssBaseline, ThemeProvider } from "@mui/material";
import { RefineThemes } from "@refinedev/mui";

function App() {
  return (
    <ThemeProvider theme={RefineThemes.Blue}>
      <CssBaseline />
      <RefineSnackbarProvider>
        <Refine>
          <ThemedLayoutV2>
            <Routes>
              {/* routes */}
            </Routes>
          </ThemedLayoutV2>
        </Refine>
      </RefineSnackbarProvider>
    </ThemeProvider>
  );
}
```

### Components

**Form:**
```tsx
import { useForm, Create } from "@refinedev/mui";
import { Box, TextField } from "@mui/material";

function ProductCreate() {
  const {
    saveButtonProps,
    refineCore: { formLoading },
    register,
    formState: { errors },
  } = useForm();

  return (
    <Create isLoading={formLoading} saveButtonProps={saveButtonProps}>
      <Box component="form">
        <TextField
          {...register("name", { required: "Name is required" })}
          error={!!errors.name}
          helperText={errors.name?.message}
          label="Name"
          fullWidth
        />
        <TextField
          {...register("price")}
          label="Price"
          type="number"
          fullWidth
        />
      </Box>
    </Create>
  );
}
```

**DataGrid:**
```tsx
import { List, useDataGrid } from "@refinedev/mui";
import { DataGrid, GridColDef } from "@mui/x-data-grid";

function ProductList() {
  const { dataGridProps } = useDataGrid();

  const columns: GridColDef[] = [
    { field: "id", headerName: "ID", width: 70 },
    { field: "name", headerName: "Name", flex: 1 },
    { field: "price", headerName: "Price", type: "number" },
  ];

  return (
    <List>
      <DataGrid {...dataGridProps} columns={columns} />
    </List>
  );
}
```

---

## Mantine Integration

### Installation

```bash
npm install @refinedev/mantine @mantine/core @mantine/hooks @mantine/notifications @emotion/react
```

### Setup

```tsx
import { Refine } from "@refinedev/core";
import { ThemedLayoutV2, RefineThemes, notificationProvider } from "@refinedev/mantine";
import { MantineProvider } from "@mantine/core";
import { Notifications } from "@mantine/notifications";

function App() {
  return (
    <MantineProvider theme={RefineThemes.Blue}>
      <Notifications />
      <Refine notificationProvider={notificationProvider}>
        <ThemedLayoutV2>
          <Routes>
            {/* routes */}
          </Routes>
        </ThemedLayoutV2>
      </Refine>
    </MantineProvider>
  );
}
```

### Components

Similar API to Ant Design and MUI:

```tsx
import { useForm, Create } from "@refinedev/mantine";
import { TextInput, NumberInput } from "@mantine/core";

function ProductCreate() {
  const { saveButtonProps, getInputProps } = useForm({
    initialValues: {
      name: "",
      price: 0,
    },
  });

  return (
    <Create saveButtonProps={saveButtonProps}>
      <TextInput
        {...getInputProps("name")}
        label="Name"
        required
      />
      <NumberInput
        {...getInputProps("price")}
        label="Price"
      />
    </Create>
  );
}
```

---

## Chakra UI Integration

### Installation

```bash
npm install @refinedev/chakra-ui @chakra-ui/react @emotion/react @emotion/styled framer-motion
```

### Setup

```tsx
import { Refine } from "@refinedev/core";
import { ThemedLayoutV2, RefineThemes, notificationProvider } from "@refinedev/chakra-ui";
import { ChakraProvider } from "@chakra-ui/react";

function App() {
  return (
    <ChakraProvider theme={RefineThemes.Blue}>
      <Refine notificationProvider={notificationProvider}>
        <ThemedLayoutV2>
          <Routes>
            {/* routes */}
          </Routes>
        </ThemedLayoutV2>
      </Refine>
    </ChakraProvider>
  );
}
```

---

## Headless (No UI Framework)

Use core hooks with any UI library:

```tsx
import { useList, useCreate } from "@refinedev/core";
import { YourCustomButton, YourCustomTable } from "./your-ui";

function ProductList() {
  const { data, isLoading } = useList({ resource: "products" });
  const { mutate: create } = useCreate();

  return (
    <div>
      <YourCustomButton onClick={() => create({ resource: "products", values: {...} })}>
        Create
      </YourCustomButton>

      <YourCustomTable
        data={data?.data}
        loading={isLoading}
      />
    </div>
  );
}
```

**With TailwindCSS:**

```tsx
import { useTable } from "@refinedev/core";

function ProductList() {
  const {
    tableQueryResult,
    current,
    setCurrent,
    pageSize,
  } = useTable();

  const data = tableQueryResult.data?.data ?? [];

  return (
    <div className="container mx-auto p-4">
      <table className="min-w-full bg-white shadow-md rounded">
        <thead>
          <tr className="bg-gray-200">
            <th className="px-4 py-2">Name</th>
            <th className="px-4 py-2">Price</th>
          </tr>
        </thead>
        <tbody>
          {data.map((product) => (
            <tr key={product.id} className="border-b">
              <td className="px-4 py-2">{product.name}</td>
              <td className="px-4 py-2">${product.price}</td>
            </tr>
          ))}
        </tbody>
      </table>

      <div className="mt-4 flex justify-center">
        <button
          onClick={() => setCurrent(current - 1)}
          disabled={current === 1}
          className="px-4 py-2 bg-blue-500 text-white rounded disabled:opacity-50"
        >
          Previous
        </button>
        <span className="px-4 py-2">Page {current}</span>
        <button
          onClick={() => setCurrent(current + 1)}
          className="px-4 py-2 bg-blue-500 text-white rounded"
        >
          Next
        </button>
      </div>
    </div>
  );
}
```

---

## Comparison

| Framework | Pros | Cons | Best For |
|-----------|------|------|----------|
| **Ant Design** | Enterprise-ready, comprehensive components | Large bundle | Admin panels, dashboards |
| **Material UI** | Google Material Design, widely used | Complex API | Modern UIs, Google aesthetic |
| **Mantine** | Modern, great DX, hooks-based | Smaller ecosystem | Modern admin panels |
| **Chakra UI** | Great accessibility, composable | Smaller component library | Accessible UIs |
| **Headless** | Complete control, small bundle | More work to build | Custom designs |

✅ **CURRENT (Nov 2025):** All frameworks are actively maintained and current.

---

## Choosing a UI Framework

**Choose Ant Design if:**
- Building enterprise admin panels
- Need comprehensive component library
- Want Chinese language support
- Need complex data tables

**Choose Material UI if:**
- Following Material Design
- Want widely adopted solution
- Need DataGrid with premium features
- Large component ecosystem needed

**Choose Mantine if:**
- Want modern, hooks-based API
- Prefer TypeScript-first
- Need excellent DX
- Building modern admin UIs

**Choose Chakra UI if:**
- Accessibility is priority
- Want composable components
- Prefer utility-first approach
- Need dark mode out of box

**Choose Headless if:**
- Have custom design system
- Want complete control
- Need smallest bundle size
- Using TailwindCSS or custom CSS

---

## Summary

**UI Packages Provide:**
- ✅ Pre-built CRUD page components
- ✅ Integrated forms and tables
- ✅ CRUD action buttons
- ✅ Layout components
- ✅ Theme support

**All Share:**
- Same hooks (`useForm`, `useTable`)
- Same patterns
- Same Refine core

**You Can:**
- Mix and match (use different UI packages in different parts)
- Switch UI frameworks (core code doesn't change)
- Go headless anytime

---

## Next Steps

**See data providers:**
→ [Data Providers Guide](DATA_PROVIDERS_GUIDE.md)

**Learn patterns:**
→ [Patterns & Conventions](PATTERNS_AND_CONVENTIONS.md)

**Build features:**
→ [How-To Guide](HOW_TO_GUIDE.md)

---

**Document Version:** 1.0
**Last Updated:** November 18, 2025
