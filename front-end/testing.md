# Testing

Testing is how you keep confidence alive as a codebase grows. A suite that gives you confidence is one that catches regressions before users do, runs fast enough to be a habit, and is easy enough to maintain that it does not become the project itself.

## The testing trophy

The classic testing pyramid (many unit tests, fewer integration, fewer still E2E) was designed for backend services where the unit is a pure function. For frontend UIs, it inverts into what Kent C. Dodds calls the testing trophy:

1. **Static analysis** (TypeScript strict mode, ESLint) — catches whole classes of bugs before runtime, at nearly zero cost.
2. **Integration tests** (largest layer) — render components with their real hooks, real queries, and MSW-mocked API. These catch the bugs that matter.
3. **Unit tests** (smaller) — test pure logic: utility functions, parsers, validation schemas, custom hooks with complex state machines.
4. **End-to-end tests** (few, targeted) — full browser runs of the 5–10 critical user journeys. Not "click every button."
5. **Visual regression** (selective) — screenshot diffs for design-system components and key pages.

The key insight: integration tests give you the most confidence per line of test code in frontend applications. Stop testing components in isolation with all dependencies mocked. Test components with their real dependencies, but mock only the network layer.

## Tool stack

| Layer               | Tool                                                        |
| ------------------- | ----------------------------------------------------------- |
| Unit / Integration  | Vitest + Testing Library                                    |
| Component (browser) | Vitest browser mode + Playwright                            |
| E2E                 | Playwright                                                  |
| Visual regression   | Playwright `toHaveScreenshot` or Vitest `toMatchScreenshot` |
| API mocking         | MSW (Mock Service Worker)                                   |
| Accessibility audit | axe-core / `@axe-core/react` in tests                       |
| Type checking       | TypeScript strict mode (first line of defence)              |

### Why Vitest over Jest

Vitest is Vite-native: shares config with the build, runs 4–10× faster than Jest, supports TypeScript and ESM natively, has a Jest-compatible API. On any project using Vite (React, Vue, Svelte, Astro), Vitest is the correct default. Jest remains valid for non-Vite setups or when a large existing test suite makes migration unjustifiable.

## Vitest setup

```
npm install -D vitest @vitest/ui jsdom @testing-library/react \
  @testing-library/user-event @testing-library/jest-dom \
  @testing-library/react-hooks msw
```

```typescript path=null start=null
// vitest.config.ts
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: ['./src/test/setup.ts'],
  },
});
```

```typescript path=null start=null
// src/test/setup.ts
import '@testing-library/jest-dom/vitest';
import { server } from './mocks/server';

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

## MSW: API mocking at the network layer

Mock Service Worker intercepts `fetch` and `XHR` at the network layer — your components call `fetch()` normally, MSW intercepts before the request leaves the process. This makes tests exercise the actual data-fetching code.

```typescript path=null start=null
// src/test/mocks/handlers.ts
import { http, HttpResponse } from 'msw';

export const handlers = [
  http.get('/api/products', () => {
    return HttpResponse.json([{ id: '1', name: 'Widget', price: 29, inStock: true }]);
  }),

  http.post('/api/search', async ({ request }) => {
    const { query } = await request.json();
    return HttpResponse.json({
      results: query === 'none' ? [] : [{ id: '1', title: 'Result' }],
    });
  }),
];
```

```typescript path=null start=null
// src/test/mocks/server.ts
import { setupServer } from 'msw/node';
import { handlers } from './handlers';

export const server = setupServer(...handlers);
```

MSW works in both Node (tests) and the browser (development against mocked APIs). The same handler file serves both contexts.

## Integration tests: the core layer

Test components the way users use them. Query by role, label, and text — not by CSS class or internal state.

```typescript path=null start=null
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { SearchForm } from './SearchForm';

describe('SearchForm', () => {
  it('shows results on valid search', async () => {
    const user = userEvent.setup();
    render(<SearchForm />);

    await user.type(screen.getByRole('searchbox'), 'widget');
    await user.click(screen.getByRole('button', { name: /search/i }));

    expect(await screen.findByText(/results for "widget"/i)).toBeInTheDocument();
  });

  it('shows empty state when no results match', async () => {
    const user = userEvent.setup();
    render(<SearchForm />);

    await user.type(screen.getByRole('searchbox'), 'none');
    await user.click(screen.getByRole('button', { name: /search/i }));

    expect(await screen.findByText(/no results found/i)).toBeInTheDocument();
  });
});
```

Rules for integration tests:

- No mocking of internal state or implementation details.
- No snapshot tests of HTML output — they are too brittle and too easy to update thoughtlessly.
- Test behavior, not internals: assert on what the user sees.

## Unit tests: pure logic only

Reserve unit tests for deterministic, side-effect-free functions:

```typescript path=null start=null
import { describe, it, expect } from 'vitest';
import { formatCurrency, calculateDiscount } from './utils';

describe('formatCurrency', () => {
  it('formats USD correctly', () => {
    expect(formatCurrency(1234.5, 'USD', 'en-US')).toBe('$1,234.50');
  });

  it('handles zero', () => {
    expect(formatCurrency(0, 'USD', 'en-US')).toBe('$0.00');
  });
});
```

### Testing custom hooks

Use `renderHook` for hooks with complex logic:

```typescript path=null start=null
import { renderHook, act } from '@testing-library/react';
import { useDebounce } from './useDebounce';

describe('useDebounce', () => {
  beforeEach(() => vi.useFakeTimers());
  afterEach(() => vi.useRealTimers());

  it('debounces value updates', () => {
    const { result, rerender } = renderHook(({ value }) => useDebounce(value, 300), {
      initialProps: { value: 'initial' },
    });

    rerender({ value: 'updated' });
    expect(result.current).toBe('initial'); // not yet

    act(() => vi.advanceTimersByTime(300));
    expect(result.current).toBe('updated'); // now
  });
});
```

## E2E tests with Playwright

Playwright supports Chromium, Firefox, and WebKit in parallel. It is the correct choice for E2E in 2026.

### Setup

```bash path=null start=null
npm init playwright@latest
```

```typescript path=null start=null
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  fullyParallel: true,
  retries: process.env.CI ? 2 : 0,
  reporter: process.env.CI ? 'github' : 'html',
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },
    { name: 'webkit', use: { ...devices['Desktop Safari'] } },
    { name: 'mobile', use: { ...devices['Pixel 7'] } },
  ],
  webServer: {
    command: 'npm run dev',
    port: 3000,
    reuseExistingServer: !process.env.CI,
  },
});
```

### Writing E2E tests

Use role-based selectors wherever possible. They test the accessible name, not the implementation:

```typescript path=null start=null
import { test, expect } from '@playwright/test';

test('user can complete checkout', async ({ page }) => {
  await page.goto('/products');

  await page.getByRole('button', { name: 'Add to Cart' }).first().click();
  await expect(page.getByTestId('cart-count')).toHaveText('1');

  await page.getByRole('link', { name: 'Checkout' }).click();
  await page.getByLabel('Email').fill('test@example.com');
  await page.getByLabel('Address').fill('123 Test St');
  await page.getByRole('button', { name: 'Place Order' }).click();

  await expect(page.getByRole('heading', { name: 'Order Confirmed' })).toBeVisible();
});
```

### When to use `data-testid`

Prefer role selectors. Use `data-testid` only when:

- Text content is dynamic (counts, timestamps).
- No meaningful accessible role exists.
- Elements are visually identical but semantically different.

Never use `data-testid` as the primary selector on elements that have meaningful roles or labels.

### Flakiness

Flaky tests are worse than no tests — they erode trust in the suite. Eliminate flakiness with:

- Playwright's built-in auto-waiting (do not use `waitForTimeout`).
- `expect(element).toBeVisible()` instead of fixed delays.
- Stable test data that does not depend on external state.
- Running each test 10 times (`--repeat-each 10`) to surface flakiness before committing.

## Visual regression testing

Visual regression catches unintended CSS changes. Playwright has built-in screenshot comparison:

```typescript path=null start=null
test('homepage visual regression', async ({ page }) => {
  await page.goto('/');
  await page.waitForLoadState('networkidle');

  await expect(page).toHaveScreenshot('homepage.png', {
    animations: 'disabled',
    maxDiffPixelRatio: 0.01, // 1% of pixels can differ
  });
});
```

### Handling dynamic content

Mask elements with time-sensitive or user-specific content:

```typescript path=null start=null
await expect(page).toHaveScreenshot('dashboard.png', {
  mask: [page.locator('[data-testid="timestamp"]'), page.locator('[data-testid="user-avatar"]')],
});
```

### CI consistency

Screenshots taken on macOS will not match those taken on Linux CI. Solve this with a Playwright Docker container, which gives deterministic rendering across all machines:

```bash path=null start=null
# Run a Playwright server in Docker
docker compose up playwright

# Connect tests to it
PW_TEST_CONNECT_WS_ENDPOINT=ws://127.0.0.1:3100 npx vitest run
```

Commit the baseline screenshots to the repository. Failing visual tests should require deliberate review before updating.

## Accessibility testing

Automated tools catch 30–40% of accessibility issues. Always run them; never rely on them exclusively.

### In integration tests

```typescript path=null start=null
import { render } from '@testing-library/react';
import { axe, toHaveNoViolations } from 'jest-axe';

expect.extend(toHaveNoViolations);

test('LoginForm has no accessibility violations', async () => {
  const { container } = render(<LoginForm />);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### In Playwright E2E

```typescript path=null start=null
import AxeBuilder from '@axe-core/playwright';

test('homepage has no accessibility violations', async ({ page }) => {
  await page.goto('/');
  const results = await new AxeBuilder({ page }).analyze();
  expect(results.violations).toEqual([]);
});
```

Run these on every pull request. A zero-violation policy prevents the baseline from degrading.

## What cannot be automated

Automated tools cannot verify:

- Whether `alt` text describes the image's purpose correctly.
- Whether error messages are actually helpful to the user.
- Whether heading hierarchy reflects content organization.
- Whether keyboard sequence is logical.
- Whether screen reader announcements are useful.

Budget manual accessibility review on every significant change. The manual checklist in `accessibility.md` covers this.

## Testing React Server Components

RSC testing runs in a Node environment and asserts on rendered output:

```typescript path=null start=null
import { render } from '@testing-library/react';
import { ProductPage } from './ProductPage';

// Mock the server-side data fetch
vi.mock('./lib/products', () => ({
  getProduct: vi.fn().mockResolvedValue({ id: '1', name: 'Widget', price: 29 }),
}));

test('renders product name and price', async () => {
  const { findByText } = render(await ProductPage({ params: { id: '1' } }));
  expect(await findByText('Widget')).toBeInTheDocument();
  expect(await findByText('$29.00')).toBeInTheDocument();
});
```

RSC testing conventions are still stabilizing. The pattern above applies to Next.js App Router components; other frameworks have their own approaches.

## CI integration

Wire all test layers into CI before merging. A minimal pipeline:

```yaml path=null start=null
# .github/workflows/test.yml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '22' }
      - run: npm ci
      - run: npm run typecheck # TypeScript
      - run: npm run lint # ESLint
      - run: npm run test # Vitest unit + integration
      - run: npx playwright install --with-deps
      - run: npm run test:e2e # Playwright
      - run: npm run test:a11y # axe E2E accessibility
```

Fail the build on any failing test. Never commit a green CI that has skipped or disabled tests.

## What to keep versus skip

**Keep:**

- Critical multi-page journeys: auth, checkout, onboarding.
- API contract handling with realistic MSW-backed responses.
- Pure logic, data transformations, custom hooks.
- User-visible behavior and interaction flows.
- Accessibility checks on every component.

**Delete or avoid:**

- Constants, config literals, and framework defaults.
- Component private state and implementation details.
- Third-party library internals you do not control.
- Styling class assertions (use screenshot diffs instead).
- Tests that mock so heavily they only verify mock behavior.

## The standard for a good test suite

A small number of well-written tests beats high coverage of shallow tests. Coverage percentage is a proxy metric. The real metric is: how many real bugs did the suite prevent from shipping? Track that instead.
