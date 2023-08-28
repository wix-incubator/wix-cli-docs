# Testing

Testing is essential for creating and keeping up functional Wix CLI code. With Wix CLI, you can easily use popular tools like Jest and Vitest for component tests.

These testing frameworks let you define expected behaviors for your code in different scenarios and then compare them with the actual outcomes.

In the sections below, we'll show you how to set up specific testing frameworks with Wix CLI.

## Vitest

Vitest is a blazing fast unit test framework powered by Vite ([Learn more about Vitest](https://vitest.dev/guide/why.html)).

Following Vitest's [getting started guide](https://vitest.dev/guide/), you can start by installing Vitest:

```
yarn install -D vitest
```

Also, install `jsdom` which we'll need for simulating a browser-like environment:

```
yarn install -D jsdom
```

Then, create the following `vitest.config.mjs` file at the root of your application:

```javascript
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    environment: 'jsdom',
  },
});
```

Next, add the following script to your `package.json`:

```diff
{
    "scripts": {
+     "test:component": "vitest"
    }
}
```

We recommend you use [Testing Library](https://testing-library.com/) for testing your UI components. To set it up, start by installing it:

```
yarn install -D @testing-library/jest-dom @testing-library/react
```

Then, add the following to your `vitest.config.mjs`:

```diff
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    environment: 'jsdom',
+   setupFiles: ['./src/test-setup.ts'],
  },
});
```

Finally, create this file which sets up [Testing Library](https://testing-library.com/):

```typescript
import { expect} from 'vitest';
import matchers from '@testing-library/jest-dom/matchers';
import '@testing-library/jest-dom/vitest';

expect.extend(matchers);
```

Once all this has been set up, here's a example test for a dashboard page:

```tsx
import React from 'react';
import { render, screen } from '@testing-library/react';
import { expect, it } from 'vitest';
import Page from './page.js';

it('renders', () => {
  render(<Page />, { legacyRoot: true });
  expect(screen.getByText('Page Title')).toBeInTheDocument();
});
```

## Jest

[Jest](https://jestjs.io/) is a delightful JavaScript Testing Framework with a focus on simplicity.

To set it up, start by installing its relevant dependencies:

```
yarn install -D jest jest-environment-jsdom @jest/globals ts-jest jsdom
```

Then, create the following `jest.config.js` file at the root of your application:

```javascript
/** @type {import('jest').Config} */
module.exports = {
  testEnvironment: 'jsdom',
  moduleNameMapper: {
    '^(?!.*(\\.st\\.css))(\\.{1,2}/.*)\\.js$': '$2',
    '\\.(css|scss)$': 'identity-obj-proxy',
  },
  extensionsToTreatAsEsm: ['.ts', '.tsx'],
  transform: {
    '^.+\\.(mt|t|cj|j)s?x$': [
      'ts-jest',
      {
        useESM: true,
      },
    ],
  },
};
```

This setup configures Jest with TypeScript support, as well as how to handle CSS imports. It also configures it to set up a browser-like environment with `jsdom`.

Next, add the following script to your `package.json`:

```diff
{
    "scripts": {
+     "test:component": "yarn node --experimental-vm-modules $(yarn bin jest)"
    }
}
```

We recommend you use [Testing Library](https://testing-library.com/) for testing your UI components. To set it up, start by installing it:

```
yarn install -D @testing-library/jest-dom @testing-library/react
```

Then, add the following to your `jest.config.js`:

```diff
/** @type {import('jest').Config} */
module.exports = {
+  setupFilesAfterEnv: ['<rootDir>/src/test-setup.ts'],
  testEnvironment: 'jsdom',
  moduleNameMapper: {
    '^(?!.*(\\.st\\.css))(\\.{1,2}/.*)\\.js$': '$2',
    '\\.(css|scss)$': 'identity-obj-proxy',
  },
  extensionsToTreatAsEsm: ['.ts', '.tsx'],
  transform: {
    '^.+\\.(mt|t|cj|j)s?x$': [
      'ts-jest',
      {
        useESM: true,
      },
    ],
  },
};
```

Finally, create this file which sets up [Testing Library](https://testing-library.com/):

```typescript
import { expect } from '@jest/globals';
import matchers from '@testing-library/jest-dom/matchers';
import '@testing-library/jest-dom/jest-globals.js';

expect.extend(matchers);

```

Once all this has been set up, here's a example test for a dashboard page:

```tsx
import React from 'react';
import { render, screen } from '@testing-library/react';
import { expect, it } from '@jest/globals';
import Page from './page.js';

it('renders', () => {
  render(<Page />, { legacyRoot: true });
  expect(screen.getByText('Page Title')).toBeInTheDocument();
});
```
