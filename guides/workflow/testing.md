# Testing

Testing is essential for creating and maintaining your app code. With the Wix CLI, you can easily use popular tools like [Jest](https://jestjs.io/) and [Vitest](https://vitest.dev/) for component tests.

These testing frameworks let you define expected behaviors for your code in different scenarios and then compare them with the actual outcomes.

In the sections below, you can see how to set up specific testing frameworks with the Wix CLI.

## Vitest

[Vitest](https://vitest.dev/) is a blazing fast unit test framework powered by Vite.

Following Vitest's [getting started guide](https://vitest.dev/guide/), you can start by installing Vitest:

```
yarn add --dev vitest
```

Also, install `jsdom` which you need for simulating a browser-like environment:

```
yarn add --dev jsdom
```

Then, create the following `vitest.config.mjs` file at the root of your application:

```javascript
import { defineConfig } from 'vitest/config';

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
+     "test": "vitest"
    }
}
```

We recommend you use [Testing Library](https://testing-library.com/) for testing your UI components. To set it up, start by installing it:

```
yarn add --dev @testing-library/jest-dom @testing-library/react@^12.0.0
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

Finally, create `src/test-setup.ts` which adds [Testing Library](https://testing-library.com/)'s matchers:

```typescript
import '@testing-library/jest-dom/vitest';
```

Once all this has been set up, here's a example test for a dashboard page:

```tsx
import React from 'react';
import { render, screen } from '@testing-library/react';
import { expect, it } from 'vitest';
import Page from './page';

it('renders', () => {
  render(<Page />);
  expect(screen.getByText('Page Title')).toBeInTheDocument();
});
```

## Jest

[Jest](https://jestjs.io/) is a delightful JavaScript Testing Framework with a focus on simplicity.

To set up Jest, start by installing its relevant dependencies:

```
yarn add --dev jest jest-environment-jsdom jsdom identity-obj-proxy @swc/core @swc/jest
```

Then, create the following `jest.config.js` file at the root of your application:

```javascript
/** @type {import('jest').Config} */
module.exports = {
  // Creates a browser-like environment
  // https://jestjs.io/docs/configuration/#testenvironment-string
  testEnvironment: 'jsdom',

  // Makes CSS and image imports act like they do outside of tests
  // https://jestjs.io/docs/configuration/#modulenamemapper-objectstring-string--arraystring
  moduleNameMapper: {
    '\\.(css|scss)$': 'identity-obj-proxy',
    '\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga)$':
      '<rootDir>/__mocks__/fileMock.js',
  },

  // Add TypeScript extensions
  // https://jestjs.io/docs/configuration/#extensionstotreatasesm-arraystring
  extensionsToTreatAsEsm: ['.ts', '.tsx'],

  // Transform TypeScript files before running them
  // https://jestjs.io/docs/configuration/#transform-objectstring-pathtotransformer--pathtotransformer-object
  transform: {
    '^.+\\.(mt|t|cj|j)s?x$': ['@swc/jest'],
  },
};
```

This setup configures Jest with TypeScript support, defines how to handle CSS imports, and sets up a browser-like environment with `jsdom`. To make sure Jest can handle images and other files, add the following file `__mocks__/fileMock.js`:

```javascript
module.exports = 'test-file-stub';
```

Next, add the following script to your `package.json`:

```diff
{
    "scripts": {
+     "test": "yarn node --experimental-vm-modules $(yarn bin jest)"
    }
}
```

We recommend you use [Testing Library](https://testing-library.com/) for testing your UI components. To set it up, start by installing it:

```
yarn add --dev @testing-library/jest-dom @testing-library/react@^12.0.0
```

Then, add the following to your `jest.config.js`:

```diff
/** @type {import('jest').Config} */
module.exports = {
+  // Sets up @testing-library
+  // https://jestjs.io/docs/configuration/#setupfilesafterenv-array
+  setupFilesAfterEnv: ['<rootDir>/src/test-setup.ts'],

  // Creates a browser-like environment
  // https://jestjs.io/docs/configuration/#testenvironment-string
  testEnvironment: 'jsdom',

  // Makes CSS and image imports act like they do outside of tests
  // https://jestjs.io/docs/configuration/#modulenamemapper-objectstring-string--arraystring
  moduleNameMapper: {
    '\\.(css|scss)$': 'identity-obj-proxy',
    '\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga)$':
      '<rootDir>/__mocks__/fileMock.js',
  },

  // Add TypeScript extensions
  // https://jestjs.io/docs/configuration/#extensionstotreatasesm-arraystring
  extensionsToTreatAsEsm: ['.ts', '.tsx'],

  // Transform TypeScript files before running them
  // https://jestjs.io/docs/configuration/#transform-objectstring-pathtotransformer--pathtotransformer-object
  transform: {
    '^.+\\.(mt|t|cj|j)s?x$': ['@swc/jest'],
  },
};
```

Finally, create `src/test-setup.ts` which adds [Testing Library](https://testing-library.com/)'s matchers:

```typescript
import '@testing-library/jest-dom';
```

Once all this has been set up, here's a example test for a dashboard page:

```tsx
import React from 'react';
import { render, screen } from '@testing-library/react';
import Page from './page';

it('renders', () => {
  render(<Page />);
  expect(screen.getByText('Page Title')).toBeInTheDocument();
});
```
