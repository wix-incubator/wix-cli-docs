---
articleType: task
tags: 
  extensions: 
    - embeddedScript
  devTools: 
    - cli
    - devCenter
---

# Set up an Embedded Script Extension Using the Wix CLI

An embedded script is an app extension that injects an HTML code fragment into the DOM of your users' sites. Unlike other extensions, embedded scripts are not fully configured during app installation, and require an additional setup step to embed the code fragment in the sites.

Follow the instructions below to:

1. Define an embedded script extension for a Wix CLI app.
2. Implement a method to embed the code fragment.

Once this task is complete, your app will have an embedded script extension that injects its HTML code fragment into the DOM of your users' sites.

## Before you begin

* You must have an app project that was [created using the Wix CLI](https://dev.wix.com/docs/build-apps/developer-tools/cli/get-started/quick-start).

## Step 1 - Create a folder to contain your embedded script

1. Navigate to your project's `src` folder and create a new folder named `site`.
2. Inside the `site` folder create a new folder called `embedded-scripts`.
3. Inside the `embedded-scripts` folder create a folder named `<your-script-name>`.

`<your-script-name>` will contain all the files for your embedded script.

Your file structure should look like this:

  ```tsx
  .
  └── <your-embedded-script-app>/
      └── src/
          └── site/
              └── embedded-scripts/
                  └── <your-script-name>
  ```

## Step 2 - Add an HTML file containing your script's code

Inside the `<your-script-name>` folder, create a file named `embedded.html` containing the code you wish to inject. Your code will be added to the page's head or body depending on your configuration.

Your file structure should look like this:

  ```tsx
  .
  └── <your-embedded-script-app>/
      └── src/
          └── site/
              └── embedded-scripts/
                  └── <your-script-name>/
                      └── embedded.html
  ```

In your HTML code, you can:
- Reference local files. (See [Referencing local files in your HTML code](#referencing-local-files-in-your-html-code-optional))
- Use dynamic parameters. (See [Using dynamic parameters in your HTML code](#using-dynamic-parameters-in-your-html-code-optional))
- Add global CSS. (See [Add global CSS to your HTML code](#add-global-css-to-your-html-code-optional))

## Step 3 - Add configuration details for your embedded script

Inside the `<your-script-name>` folder, create a file named `embedded.json`. 

Your file structure should look like this:
  ```tsx
  .
  └── <your-embedded-script-app>/
      └── src/
          └── site/
              └── embedded-scripts/
                  └── <your-script-name>/
                      ├── embedded.html
                      ├── embedded.json
                      └── params.dev.json
  ```

This file must have the following structure:

  ```tsx
  {
    "id": "1347ffed-668c-4133-9d90-510f58f02c33",
    "name": "console-logger",
    "scriptType": "FUNCTIONAL",
    "placement": "HEAD"
  }
  ```

* `id`  is a unique identifier for your script. For example, a randomly generated GUID.
* `name` is the name of your script as it will appear in the [Wix Developers Center](https://dev.wix.com/apps/my-apps). It can only contain letters and the hyphen (-) character.
* `scriptType` is an enum used by Wix's Cookie Consent Banner tool to determine whether site visitors consent to having your script run during their visit. Possible values are:
  * `"ESSENTIAL"`: Enables site visitors to move around the site and use essential features like secure and private areas crucial to the functioning of the site.
  * `"FUNCTIONAL"`: Remembers choices site visitors make to improve their experience, such as language.
  * `"ANALYTICS"`: Provides statistics to the site owner on how visitors use the site, such as which pages they visit. This helps improve the site by identifying errors and performance issues.
  * `"ADVERTISING"`: Provides visitor information to the site owner to help market their products, such as data on the impact of marketing campaigns, re-targeted advertising, and so on.

  > **About types**
  >  
  > An embedded script must have a type. If your script falls into more than one type, choose the option closest to the bottom of the list above. For example, if your script has **Advertising** and **Analytics** aspects, choose **Advertising** as its type. It's unlikely that you'll need to mark it as **Essential** – if you think you need to use this, [get in touch with us](https://devforum.wix.com/en/contact).

* `placement` is an enum indicating where in the page's DOM the HTML code will be injected. Possible values are:
  * `"HEAD"`: Injects the code between the page's `<head>` and `</head>` tags.
  * `"BODY_START"`: Injects the code immediately after the page's opening `<body>` tag.
  * `"BODY_END"`: Injects the code immediately before the page's closing `</body>` tag.

## Referencing local files in your HTML code (Optional)

Wix will host and deploy every file in your project unless you specify otherwise, including any that you add. Your HTML code can reference these files using a relative path. Your HTML code can reference these files using a relative path.

When referencing local files in a `<script>` tag, the tag needs to have `type="module"`.

For example, to reference a file named `local-script.js` in the same directory as `embedded.html`, use the following code:

  ```tsx
  <script type = "module" src = "./local-script.js"></script>
  ```
>**Note:** TypeScript files are supported.

## Using dynamic parameters in your HTML code (Optional)

You can embed dynamic parameters in your code to inject custom information per site.

For example:

  ```tsx
  <meta name = "google-tag" id = "{{googleTag}}"></meta>
  <script> console.log("Hello {{userName}} from the CLI.");</script>
  ```

Dynamic parameters must be:

* Wrapped in double curly braces. For example, `{{userName}}`.
* Within a quote (") to avoid code evaluation.
* Set using the [Embedded Scripts API](https://dev.wix.com/api/rest/app-management/apps/embedded-scripts/introduction). This is explained in detail in step 5.
* Made up only of letters. (No special characters or spaces.)

Dynamic parameter values must be strings.

### Specify dynamic parameter values to use during development

In production, dynamic parameter values are set after installation by calling the Embed Script endpoint explained in Step 5.

When testing in your local development environment, you can specify values to assign to these parameters as follows:

1. Inside the `<your-script-name>` folder, create a file named `params.dev.json`.

   Your file structure should look like this:

    ```tsx
    .
    └── <your-embedded-script-app>/
        └── src/
            └── site/
                └── embedded-scripts/
                    └── <your-script-name>/
                        ├── embedded.html
                        └── params.dev.json
    ```

2. Make sure this file includes an object containing key-value pairs for each of your dynamic parameters.

   For example, for the code:

    ```tsx
    <script> console.log("Hello {{userName}} from the CLI. Your Google Tag ID is: {{googleTagId}}."); </script>
    ```

    Requires a params.dev.json file in the following format:

    ```tsx
    {
      "userName": "Jerry",
      "googleTagId": "GT-XXXXXXXXX"
    }
    ```

Make sure the keys are the dynamic parameter names in quotes. The values will be assigned to the parameters when testing your script.

## Add global CSS to your HTML code (Optional)

You can add CSS directly to your `embedded.html` file, or you can reference a CSS stylesheet with a link. For example:

  ```tsx
  <link rel="stylesheet" href="./<your-css-file>.css"/>
  ```

This CSS applies to your site globally. For example, the following code would make the background of every page of your site red:

  ```tsx
  #c1dmp {
    background: red;
  }
  ```

## Prepare your app for production

To finish setting up your embedded script, either you or the site owner must call the [Embed Script](https://dev.wix.com/docs/rest/api-reference/app-management/apps/embedded-scripts/embed-script) endpoint to embed your script and update the values of the dynamic parameters in each app instance.

### Prompting the site owner to embed the code (recommended)

If your app has a dashboard page, you have the option to shift responsibility for this last configuration step onto site owners.

Site owners can call the [Embed Script](https://dev.wix.com/docs/rest/api-reference/app-management/apps/embedded-scripts/embed-script) endpoint using the `fetch` method from the [Wix React SDK](https://dev.wix.com/docs/sdk/api-reference/sdk-react/setup).

>**Note**: If an app has a dashboard page and an embedded script extension, site owners will automatically be directed to the app's dashboard page after installing the app.

This API call is also used to specify the value of any dynamic parameters. For more information about using dynamic parameters see (See [Using dynamic parameters in your HTML code](#using-dynamic-parameters-in-your-html-code-optional)).

To use the `fetch` method in your app's dashboard page:

1. Navigate to your `page.tsx` file in `src/dashboard/pages`.
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

If your app contains more than one embedded script, you must also pass a `componentId` using the id value defined in your `embedded.json`. In this situation, your call should be in the following format:

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
If your app only has 1 embedded script, don't pass the `componentId` in the request body. This action could lead to your app breaking in production. The `componentId` is only relevant for apps with more than 1 embedded script.

</blockquote>

Ensure that the `parameters` section of the `body` contains all the dynamic parameters in your embedded script. Otherwise, you will get an error and your code will not be embedded.

### Embedding a script as an app developer

You can also call the [Embed Script](https://dev.wix.com/docs/rest/api-reference/app-management/apps/embedded-scripts/embed-script) endpoint from your server once the app is installed on a user's site. This will require an access token obtained through the [OAuth process](https://dev.wix.com/docs/build-apps/build-your-app/authentication/oauth).

## Summary

By following these instructions, you have configured an embedded script extension for your application and provided a method to embed its HTML code fragment on your users' sites. When site owners download your app and complete the setup process, your code will be injected into the DOM of every page on their site at the location you specified.
