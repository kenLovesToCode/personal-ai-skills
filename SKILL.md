---
name: nextjs-vitest-rtl-testing
description: Configure and maintain Next.js unit testing with Vitest and React Testing Library. Use when adding a test runner, creating setup/config files, writing or fixing component or hook tests, mocking Next.js APIs like next/navigation or next/image, mocking API endpoints with MSW (`http`, `HttpResponse`), improving coverage, or debugging failing unit tests in Next.js projects.
---

# Next.js Vitest RTL Testing

Set up and maintain fast, deterministic unit tests for Next.js applications with Vitest and React Testing Library.

## Workflow

1. Inspect the repo before changing anything.
2. Configure Vitest and test setup files.
3. Add or repair tests using stable patterns.
4. Run tests and fix environment or mocking issues.

## Inspect The Repo

- Detect package manager from lockfile (`pnpm-lock.yaml`, `package-lock.json`, `yarn.lock`, `bun.lockb`).
- Detect TypeScript vs JavaScript.
- Detect router usage (App Router, Pages Router, or mixed).
- Reuse existing path aliases from `tsconfig.json`/`jsconfig.json`.

## Configure Test Infrastructure

- Follow [`references/setup-checklist.md`](references/setup-checklist.md) for exact install commands and file templates.
- Prefer `vitest.config.ts` when TypeScript is enabled.
- Keep setup code in `tests/setup.ts` or `src/tests/setup.ts` and include it in `setupFiles`.
- Prefer MSW for API request mocking at network boundaries instead of stubbing `fetch` directly.
- Add package scripts for `test`, `test:watch`, and `test:coverage`.

## Write And Fix Tests

- Follow [`references/test-patterns.md`](references/test-patterns.md) for patterns:
- Render and interaction tests for React components.
- Next.js mocks (`next/navigation`, `next/image`, `next/link` as needed).
- API endpoint mocking with MSW `http` and `HttpResponse`.
- Async/data-fetching behavior with controlled handlers and fake timers when needed.
- Hook and utility tests with clear Arrange/Act/Assert structure.

## Guardrails

- Avoid snapshot-heavy tests for interactive UI; assert behavior and accessible output.
- Avoid over-mocking internals; mock network and framework boundaries.
- Keep tests isolated; reset mocks in `beforeEach`/`afterEach`.
- If Server Components are hard to unit test directly, extract pure logic into testable helpers.

## Troubleshooting

- `document is not defined`: set `environment: "jsdom"` in Vitest config.
- Alias resolution errors (for `@/`): add matching `resolve.alias` entries in Vitest config.
- `next/navigation` runtime errors: provide explicit `vi.mock("next/navigation", ...)` return values.
- Missing matcher errors (for `toBeInTheDocument`): import `@testing-library/jest-dom/vitest` in setup file.
- MSW unhandled request errors: add/adjust a matching handler or set an intentional passthrough.

## Output Expectations

- Leave the repo with runnable tests and stable commands.
- Keep configs minimal and aligned with existing project conventions.
- Add at least one passing representative test when bootstrapping from zero.
