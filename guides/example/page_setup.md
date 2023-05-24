# Dashboard Page Setup

In the Business Buddy app, the pages appear in the dashboard with the following structure.

![Sidebar structure](../images/tutorial_sidebar_structure.png)

To create this structure, we:

- Place dashboard page files in subfolders of the **dashboard/pages** folder
- Configure the sidebar in the **wix.config.json** file

## Dashboard page files

A pair of **page.json** and **page.tsx** files defines one dashboard page. The **page.json** files define the page metadata and the page **page.tsx** file defines the page UI and functionality. Our app project has two sets of these file pairs in subfolders under the **dashboard/pages** folder.

Each file pair exists in a folder that determines the route to a page. So our app has two pages with the routes **/product** and **/settings**.

## Sidebar configuration

Our app has two pages, but without any further configuration than what was described above, both pages would appear in the top level of the dashboard.

![Sidebar pages](../images/tutorial_sidebar_pages.png)

In our case, we want to configure them to show under a category in the dashboard sidebar so they look like they are part of the same app.

We configure where pages show in the sidebar in the **wix.config.json** file and the **page.json** metadata files.

### wix.config.json

In the **wix.config.json** file, we have the following dashboard configuration that creates a new category named "Business Buddy".

```json
"dashboard": {
  "sidebar": {
    "categories": [
      {
        "id": "a1efac42-a928-470d-8b82-26661b6f906b",
        "label": "Business Buddy"
      }
    ]
  }
}
```

### page.json

Now that we have a new category, we can add our pages to the category using the **page.json** metadata files. In those files, we add the following sidebar configuration to associate the pages with the category we created.

```json
"sidebar": {
  "categoryId": "a1efac42-a928-470d-8b82-26661b6f906b"
}
```

At this point, we've configured our sidebar so we see the **Products** and **Settings** pages under the **Business Buddy** category.

![Sidebar structure](../images/tutorial_sidebar_structure.png)

## Up next

Now that you understand how to set up dashboard pages, we can see how to add components to the pages to create their look and feel.
