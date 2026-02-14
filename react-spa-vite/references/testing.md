# Testing

## Vitest (Unit/Component)

Install:

```bash
pnpm add -D vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event jsdom
```

Add test config in `vite.config.ts`:

```ts
test: {
  globals: true,
  environment: 'jsdom',
  setupFiles: './src/test/setup.ts',
  css: true,
}
```

Create setup file:

```ts
import { afterEach, expect } from 'vitest';
import { cleanup } from '@testing-library/react';
import * as matchers from '@testing-library/jest-dom/matchers';

expect.extend(matchers);
afterEach(() => cleanup());
```

Suggested scripts:

```json
{
  "scripts": {
    "test": "vitest",
    "test:coverage": "vitest --coverage"
  }
}
```

## Playwright (E2E)

Initialize:

```bash
pnpm create playwright
```

Recommended baseline:

- `testDir: ./e2e`
- `baseURL` matching dev server
- retries/workers tuned for CI
- browser matrix only as needed

Suggested script:

```json
{
  "scripts": {
    "test:e2e": "playwright test"
  }
}
```

For production-build E2E, run tests against `pnpm preview` after `pnpm build`.
