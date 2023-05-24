# App Framework Features

The Wix App Framework is a React-based framework that allows you to build apps for Wix sites. The framework provides a way to define app components, their React based UI, styling and more.

The Wix App Framework is:

- **React-based**: Use React components to define your app's UI. You can use React components from the [Wix Design System](https://www.wixdesignsystem.com/) to build your app's UI or bring your own components.
- **Boilerplate-free**: You don't have to write any boilerplate code. Focus on your app's business logic and let the framework handle the rest.
- **Declarative**: Define your app's components and their UI in a declarative manner using React and the app manifest.

## Vite

The Wix App Framework is based on [Vite](https://vitejs.dev) as the toolchain for building your app's code. Vite is a fast and opinionated web dev build tool that focuses on providing a great developer experience.

You can use all the Vite features, including:

- [TypeScript Support](https://vitejs.dev/guide/features.html#typescript)
- [CSS Pre-processors](https://vitejs.dev/guide/features.html#css-pre-processors)
- [CSS Modules](https://vitejs.dev/guide/features.html#css-modules)
- [PostCSS](https://vitejs.dev/guide/features.html#postcss)
- [Static Assets](https://vitejs.dev/guide/features.html#static-assets)
- [Hot Module Replacement](https://vitejs.dev/guide/features.html#hot-module-replacement)

We suggest you read the [Vite Features Guide](https://vitejs.dev/guide/features.html) to learn more about Vite.

## App manifest

The app manifest is a declarative definition of your app's components and their UI, including:

- Dashboard pages
- Sidebar configurations
- App's settings

The app manifest is created from your local project files. For example, the app's dashboard pages are defined by the different `page.json` files in the `src/dashboard/pages` folder and the app's sidebar categories are defined by the `wix.config.json` file in the root of the app project.

- When you build your project, the app manifest is located at `dist/devcenter/app-manifest.json`
- When you run your app locally, the app manifest is managed in-memory and updated on the fly when you make changes

### wix.config.json

The `wix.config.json` file in the root of the app project defines basic information about your app, such as its `appId`. The file also defines app level configurations like `dashboard.sidebar.categories`.

To learn more about sidebar categories in the `wix.config.json` file, see [Sidebar Categories](./dashboard_pages.md#sidebar-categories).
