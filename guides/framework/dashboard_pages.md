# Dashboard Pages

Wix Apps can extend the [Wix Dashboard](https://support.wix.com/en/article/about-your-wix-dashboard) by adding new pages to the dashboard. You can create a dashboard page to help users manage their sites or businesses. Dashboard pages aren’t part of a live site, so site visitors will never see them.

## Dashboard page files

Within your app project, the source code for dashboard pages is found in the `src/dashboard/pages` folder.

Each page is defined in its own subfolder that contains two files:

- `page.json`: Defines the [page metadata](#page-metadata-pagejson)
- `page.tsx`: Defines the [page content](#page-content-pagetsx)

The location of the page's subfolder determines the [route](#page-routes) to the page.

### Page metadata (page.json)

The `page.json` file contains the dashboard page metadata.

The page metadata is defined using the following schema, shown here as a TypeScript type:

```ts
type BackOfficePageConfig = {
  id: string;
  title: string;
  additionalRoutes?: string[];
  sidebar?: {
    disabled?: boolean;
    categoryId?: string;
    priority?: number;
    whenActive?: {
      selectedPageId?: string;
      hideSidebar?: boolean;
    };
  };
};
```

Here's an example metadata definition:

```json
{
  "id": "15a6e3ad-620e-416b-840d-367be43a4fd8",
  "title": "My CLI App Page",
  "sidebar": {
    "categoryId": "041e3b2a-d25b-47ce-8162-dfdfe0e64785",
    "whenActive": {
      "hideSidebar": true
    }
  }
}
```

#### id

The page ID ([GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)). The ID is used to register the page in Wix Developers Center. The ID must be unique across all pages in the app.

#### title

The page title. The title is used as the browser tab title and as the dashboard sidebar label if the page is configured to appear in the sidebar.

#### additionalRoutes (optional)

A list of routes that lead to this page (in addition to the main route defined by the [file path convention](#page-routes)). This is useful if you want to support multiple routes for the same page or for backwards compatibility.

For example, if you want both `/products` and `/products/list` to lead to the same page.

#### sidebar (optional)

An object that defines the behavior of this page in the dashboard sidebar.

##### sidebar.disabled (optional)

Whether the page is shown in the sidebar. If `true`, the page is not shown in the sidebar. Defaults to `false`.

##### sidebar.categoryId (optional)

The ID of the category to associate this page with, as defined in the [sidebar categories configuration](#category-configuration).

##### sidebar.priority (optional)

The priority of the page entry in the sidebar. Determines the page location in relation to other app pages. Smaller numbers mean higher priority.

##### sidebar.whenActive.selectedPageId (optional)

When the page is the currently active page, this setting determines the page that is selected in the sidebar.

You can use this you have a page that is a nested page and doesn’t have an entry in the sidebar, but you want the sidebar to display some page as active.

For example, you might have a number of pages for a specific products. You don't want these pages cluttering up the sidebar, so you set them not to display there. However, when one of the product pages is active, you might want the products list page to be selected in the sidebar.

Defaults to the current page.

##### sidebar.whenActive.hideSidebar (optional)

Whether the sidebar will be shown when the page is active. If `true`, the sidebar is hidden when the page is active. Defaults to `false`.

Hiding the sidebar creates a more immersive experience by gaining more screen real estate.

### Page content (page.tsx)

The `page.tsx` file contains the dashboard page content.

The content is defined as a [React](https://react.dev/) component that renders when the page is active.

Inside a dashboard page component you can use:

- The [Dashboard SDK](https://dev.wix.com/docs/client/api-reference/dashboard-sdk/intro) to navigate users to pages in the dashboard, display modals, and send users alerts and updates using toasts.
- The [Wix Design System](https://www.docs.wixdesignsystem.com/?path=/story/getting-started--about) to use the same React components Wix uses to build its own dashboard pages.

### Page Routes

The location of a dashboard page files within the `src/dashboard/pages` folder determines the route to the page. The route is the path that is appended to the dashboard base URL to access the dashboard page. Therefore, you should name your page folders using URL-friendly names.

To create longer routes, nest `page.json` files inside of subfolders.

For example, consider this folder structure:

```bash
src
└── dashboard
    └── pages
        ├── products
        │   ├── page.json
        │   ├── page.tsx
        │   └── analytics
        │       ├── page.json
        │       └── page.tsx
        ├── orders
        │   ├── page.json
        │   └── page.tsx
        ├── page.json
        └── page.tsx
```

This app adds 4 pages to the dashboard:

- Index page (`dashboard/pages/page.json`) - Route: `/`
- Products page (`dashboard/pages/products/page.json`) - Route: `/products`
- Analytics page (`dashboard/pages/products/analytics/page.json`) - Route: `/products/analytics`
- Orders page (`dashboard/pages/orders/page.json`) - Route: `/orders`

## Sidebar Categories

The dashboard sidebar is divided into categories. Each category is defined by a category ID and a category label. The category ID is used to associate pages with a specific category. The category label is used as the display label of the category under which all the pages that belong to that category can be found.

![Sidebar categories](../../media/dashboard_sidebar_structure.png)

### Category Configuration

The `wix.config.json` file in the root of the app project defines your app's sidebar categories.

The categories are defined using the following schema, shown here as a TypeScript type:

```ts
type WixConfigJSON = {
  // ... other configurations
  dashboard?: {
    sidebar?: {
      categories?: Array<SidebarCategory>;
    };
  };
};

type SidebarCategory = {
  id: string;
  label: string;
  priority?: number;
};
```

Here's an example configuration:

```json
"dashboard": {
  "sidebar": {
    "categories": [{
      "id": "041e3b2a-d25b-47ce-8162-dfdfe0e64785",
      "label": "My CLI App"
    }]
  }
}
```

#### SidebarCategory

##### id

The category ID ([GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)). The ID is used as a reference to the category from [page configurations](#sidebarcategoryid-optional). The ID must be unique across all categories in the app.

##### label

The text to display as the category label in the sidebar.

##### priority (optional)

The priority of the category in the sidebar. Determines the category location in relation to other app categories. Smaller numbers mean higher priority.

## Add a new dashboard page

To add a new dashboard page to your app:

1. Create a new subfolder under `src/dashboard/pages` that will determine the route of your page.
1. Add a new set of `page.json` and `page.tsx` [dashboard page files](#dashboard-page-files) to the subfolder you created.
1. Add the page metadata to the `page.json` file.
1. Add the page code to the `page.tsx` file.

## Delete a dashboard page

To delete an existing dashboard page from you app, delete the subfolder under `src/dashboard/pages` that contains your [dashboard page's files](#dashboard-page-files).

> **Note**: If you've already [created a version](../workflow/app_versions_and_deployment.md#deployable-version) of your app, deleting a dashboard page's files from your project will not remove the registration of the dashboard page from the Wix Developers Center. To remove the registration, [create a new version](../workflow/app_versions_and_deployment.md#deployable-version) after deleting the [dashboard page's files](#dashboard-page-files).
