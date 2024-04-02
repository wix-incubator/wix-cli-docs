---
articleType: task
tags: 
  extensions: 
    - embeddedScript
  interfaces:
    - site
    - dashboard
  frameworks: 
    - cli
  apis:
    - rest
    - sdk
---

# Set up an Embedded Script Extension

An embedded script is an app extension that injects an HTML code fragment into the DOM of your users' sites. Unlike other extensions, embedded scripts are not fully configured during app installation, and require an additional setup step to embed the code fragment in the sites.

Follow the instructions below to:

1. Create an embedded script extension for a Wix CLI app.
2. Prepare your app for production.
3. Build and deploy your app.

Once this task is complete, your app will have an embedded script extension that injects its HTML code fragment into the DOM of every page of your users' sites.

Before you begin, you must have an app project that was [created using the Wix CLI](https://dev.wix.com/docs/build-apps/developer-tools/cli/get-started/quick-start).

## Step 1 | Create the extension

In the terminal:
1. Navigate to your project repo.
2. Run the following command and follow the prompts to create an embedded script extension:

```tsx
npm run generate
```

During the creation process you will be prompted to select a **script type** and a **placement**. For this guide, you can make any selection. For more information, see the [script type and placement options](./embedded-script-extension-files-and-code.md#embeddedjson).

Upon completion, the extension files will be created in your app directory with the following structure:

  ```tsx
  .
  └── <your-app-name>/
      └── src/
          └── site/
              └── embedded-scripts/
                  └── <your-script-name>/
                      ├── embedded.html
                      ├── embedded.json
                      └── params.dev.json
  ```

+ [**embedded.html**](./embedded-script-extension-files-and-code.md#embeddedhtml) - This file contains the HTML code fragment for your embedded script.
+ [**embedded.json**](./embedded-script-extension-files-and-code.md#embeddedjson) - This file contains information about your embedded script.
+ [**params.dev.json**](./embedded-script-extension-files-and-code.md#paramsdevjson) - This file contains values for dynamic parameters to use during testing.

> **Note:** This step has covered the basics of setting up an embedded script extension. For more information about how you can customize your embedded script, read [Embedded Script Extension Files and Code](./embedded-script-extension-files-and-code.md)

## Step 2 | Prepare your app for production

To finish setting up your embedded script, either you or the site owner must call the [Embed Script](https://dev.wix.com/docs/rest/api-reference/app-management/apps/embedded-scripts/embed-script) endpoint to embed your script and update the values of the dynamic parameters in each app instance.

#### Prompting the site owner to embed the code (recommended)

If your app has a dashboard page, you have the option to shift responsibility for this last configuration step onto site owners.

>**Note**: If an app has a dashboard page and an embedded script extension, site owners will automatically be directed to the app's dashboard page after installing the app.

Site owners can call the [Embed Script](https://dev.wix.com/docs/rest/api-reference/app-management/apps/embedded-scripts/embed-script) endpoint using the `fetch` method from the [Wix React SDK](https://dev.wix.com/docs/sdk/api-reference/sdk-react/setup).

This API call is also used to specify the value of any dynamic parameters. For more information about using dynamic parameters see [Using dynamic parameters in your HTML code](./embedded-script-extension-files-and-code.md#using-dynamic-parameters-in-your-html-code).

To use the `fetch` method in your app's dashboard page:

1. Open the `page.tsx` file in your app’s `src/dashboard/pages` folder.
2. Add the following import statement at the top of your page:

     ```tsx
      import { useWix } from "@wix/sdk-react";
      ```

3. Inside the `Index` method, add the following code:

      ```tsx
      const { fetch } = useWix();
      ```

4. Add the `fetch` call somewhere in your code.

For example, add a call to action with instructions to click a button to complete setup for your app. Then, when the site owner clicks the button, they will call the fetch method.

If your app contains only one embedded script, the fetch method call should be in the following format:

  ```tsx
  fetch('https://www.wixapis.com/apps/v1/scripts', {
    method : 'post',
    headers : {'content-type':'application/json'},
    body : JSON.stringify({
      "properties": {
        "parameters": {
          "<your-key-1>": "<your-value-1>",
          "<your-key-2>": "<your-value-2>",
        }
      }
    })
  })
  ```

If your app contains more than one embedded script, you must also pass a `componentId` using the id value defined in your `embedded.json` file. In this situation, your call should be in the following format:

  ```tsx
  fetch('https://www.wixapis.com/apps/v1/scripts', {
    method : 'post',
    headers : {'content-type':'application/json'},
    body : JSON.stringify({
      "properties": {
        "parameters": {
          "<your-key-1>": "<your-value-1>",
          "<your-key-2>": "<your-value-2>",
        }
      },
      "componentId": <your-component-id>
    })
  })
  ```
<blockquote class="warning">

__Warning:__
If your app only has 1 embedded script, don't pass the `componentId` in the request body. This action could break your app in production. The `componentId` is only relevant for apps with more than 1 embedded script.

</blockquote>

Ensure that the `parameters` section of the `body` contains all the dynamic parameters in your embedded script. Otherwise, you will get an error and your code will not be embedded.

#### Embedding a script as an app developer

You can also call the [Embed Script](https://dev.wix.com/docs/rest/api-reference/app-management/apps/embedded-scripts/embed-script) endpoint from your server once the app is installed on a user's site. This requires an access token obtained through the [OAuth process](https://dev.wix.com/docs/build-apps/build-your-app/authentication/oauth).

## Step 3 | Build and deploy your app

Now that your app is ready for production, you can build your app and create a version in the [Wix Dev Center](https://dev.wix.com/apps/my-apps?viewId=active-apps-view). 

1. Run the following command to build your app:

  ```bash
  npm run build
  ```

2. Run the following command and follow the prompts to create an app version:

  ```bash
  npm run create-version
  ```

An app version allows you to publish an app to the [Wix App Market](https://www.wix.com/app-market) or install it on a site with a direct install URL.

For more information about the above commands, see [Wix CLI Commands for Apps](https://dev.wix.com/docs/build-apps/developer-tools/cli/wix-cli-for-apps/commands).

## Summary

By following these instructions, you have:

1. Created and configured an embedded script extension for your application.
2. Provided a method to embed your extension’s HTML code fragment on your users' sites. When site owners download your app and complete the setup process, your code will be injected into the DOM of every page on their site.
3. Built your app and created a version on the Wix Dev Center.

