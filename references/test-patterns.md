# Test Patterns

Use these patterns to keep Next.js unit tests readable and reliable.

## Component Render Test

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

function Counter() {
  return <button>Count</button>;
}

it("renders the button", async () => {
  render(<Counter />);
  await userEvent.click(screen.getByRole("button", { name: "Count" }));
  expect(screen.getByRole("button", { name: "Count" })).toBeInTheDocument();
});
```

## Mock `next/navigation`

```ts
import { vi } from "vitest";

const push = vi.fn();

vi.mock("next/navigation", () => ({
  useRouter: () => ({ push, replace: vi.fn(), back: vi.fn() }),
  usePathname: () => "/dashboard",
  useSearchParams: () => new URLSearchParams("tab=all"),
}));
```

Assert redirect behavior:

```ts
expect(push).toHaveBeenCalledWith("/login");
```

## Mock `next/image`

```ts
vi.mock("next/image", () => ({
  default: (props: React.ImgHTMLAttributes<HTMLImageElement>) => {
    return <img {...props} alt={props.alt ?? ""} />;
  },
}));
```

## Mock API Endpoints With MSW (`http`, `HttpResponse`)

```ts
import { http, HttpResponse } from "msw";
import { server } from "../tests/__mocks__/server"; // adjust path for your test file

server.use(
  http.get("*/api/items", () => {
    return HttpResponse.json({ items: [{ id: 1, name: "A" }] });
  })
);
```

Error response override:

```ts
server.use(
  http.get("*/api/items", () => {
    return HttpResponse.json(
      { message: "Internal error" },
      { status: 500 }
    );
  })
);
```

MSW lifecycle is managed in `tests/setup.ts`:

```ts
beforeAll(() => server.listen({ onUnhandledRequest: "error" }));
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

## API Mock Best Practices

- Mock at the HTTP boundary (`msw`) instead of mocking `fetch` internals.
- Use route patterns like `*/api/...` to avoid brittle host-specific tests.
- Keep default handlers in `handlers.ts`, then override per test using `server.use(...)`.
- Use `onUnhandledRequest: "error"` to catch accidental real network calls early.
- Return realistic status codes and payloads with `HttpResponse.json(...)`.
- Reset handlers and mock state after each test for isolation.

## Hook Test

```tsx
import { renderHook } from "@testing-library/react";

function useReady() {
  return true;
}

it("returns true", () => {
  const { result } = renderHook(() => useReady());
  expect(result.current).toBe(true);
});
```

## Server Component Guidance

- Avoid heavy unit tests against full Server Components.
- Extract formatting, mapping, and validation logic into pure helpers.
- Unit test helpers directly; cover end-to-end Server Component behavior with integration or E2E tests.
