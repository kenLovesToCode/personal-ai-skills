# Setup Checklist

Use this checklist to bootstrap Vitest + React Testing Library + MSW in a Next.js repo.

## 1) Install Dependencies

Choose one package manager:

```bash
npm install -D vitest jsdom msw @vitejs/plugin-react @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

```bash
pnpm add -D vitest jsdom msw @vitejs/plugin-react @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

```bash
yarn add -D vitest jsdom msw @vitejs/plugin-react @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

```bash
bun add -d vitest jsdom msw @vitejs/plugin-react @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

## 2) Create Vitest Config

Create `vitest.config.ts`:

```ts
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";
import { resolve } from "node:path";

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    setupFiles: ["./tests/setup.ts"],
    globals: true,
    clearMocks: true,
  },
  resolve: {
    alias: {
      "@": resolve(__dirname, "./src"),
    },
  },
});
```

If the project does not use `src/`, point `@` to the correct base folder.

## 3) Create MSW Handlers

Create `tests/__mocks__/handlers.ts`:

```ts
import { http, HttpResponse } from "msw";

export const handlers = [
  http.get("*/api/items", () => {
    return HttpResponse.json({ items: [{ id: 1, name: "A" }] });
  }),
];
```

Create `tests/__mocks__/server.ts`:

```ts
import { setupServer } from "msw/node";
import { handlers } from "./handlers";

export const server = setupServer(...handlers);
```

## 4) Create Setup File

Create `tests/setup.ts`:

```ts
import { beforeAll, afterEach, afterAll, vi } from "vitest";
import "@testing-library/jest-dom/vitest";
import { cleanup } from "@testing-library/react";
import { server } from "./__mocks__/server";

beforeAll(() => server.listen({ onUnhandledRequest: "error" }));

afterEach(() => {
  server.resetHandlers();
  vi.resetAllMocks();
  cleanup();
});

afterAll(() => server.close());
```

## 5) Add Package Scripts

Add to `package.json`:

```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage"
  }
}
```

## 6) Add A Smoke Test

Create `src/components/__tests__/smoke.test.tsx`:

```tsx
import { render, screen } from "@testing-library/react";

function Smoke() {
  return <h1>Ready</h1>;
}

describe("Smoke", () => {
  it("renders", () => {
    render(<Smoke />);
    expect(screen.getByRole("heading", { name: "Ready" })).toBeInTheDocument();
  });
});
```

## 7) Run Tests

Run one command:

```bash
npm test
```

```bash
pnpm test
```

```bash
yarn test
```

```bash
bun test
```
