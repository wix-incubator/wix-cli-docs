---
articleType: reference
tags: 
  extensions: 
    - embeddedScript
  interfaces:
    - site
  frameworks: 
    - cli
---

# Embedded Script Extension Files and Code

The structure of an embedded script extension in a CLI app is as follows:

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

These files can be generated in your app by running the following command and selecting **Embedded Script extension**:

```tsx
npm run generate
```

## embedded.html

The file named `embedded.html` contains the code you wish to inject. Your code will be added to the page's head or body depending on your configuration.

Your file structure should look like this:

In your HTML code, you can:
- [Reference local files](#referencing-local-files-in-your-html-code)
- [Use dynamic parameters](#using-dynamic-parameters-in-your-html-code)
- [Add global CSS](#add-global-css-to-your-html-code)

### Referencing local files in your HTML code

Wix will host and deploy every file in your project unless you specify otherwise, including any that you add. Your HTML code can reference these files using a relative path. Your HTML code can reference these files using a relative path.

When referencing local files in a `<script>` tag, the tag needs to have `type="module"`.

For example, to reference a file named `local-script.js` in the same directory as `embedded.html`, use the following code:

  ```tsx
  <script type = "module" src = "./local-script.js"></script>
  ```
>**Note:** TypeScript files are supported.

### Using dynamic parameters in your HTML code

Dynamic parameters are placeholders in your code that allow for the injection of custom information specific to each site where the code is deployed.

Dynamic parameters must be:

* Wrapped in double curly braces. For example, `{{userName}}`.
* Within a quote (") to avoid code evaluation.
* Set using the [Embedded Scripts API](https://dev.wix.com/api/rest/app-management/apps/embedded-scripts/introduction). This is explained in detail in step 5.
* Made up only of letters. (No special characters or spaces.)

Dynamic parameter values must be strings.

For example, the following code contains the dynamic parameters `googleTag` and `userName`:

  ```tsx
  <meta name = "google-tag" id = "{{googleTag}}"></meta>
  <script> console.log("Hello {{userName}} from the CLI.");</script>
  ```

See [below](#paramsdevjson) how to specify dynamic parameter values to use during development

### Adding global CSS to your HTML code

You can add CSS directly to your `embedded.html` file, or you can reference a CSS stylesheet with a link. For example:

  ```tsx
  <link rel="stylesheet" href="./your-css-file.css"/>
  ```

This CSS applies to your site globally. For example, the following code would make the background of every page of your site red:

  ```tsx
  #c1dmp {
    background: red;
  }
  ```

## embedded.json

This file contains information about your script, and must have the following structure:

  ```tsx
  {
    "id": "1347ffed-668c-4133-9d90-510f58f02c33",
    "name": "console-logger",
    "scriptType": "FUNCTIONAL",
    "placement": "HEAD"
  }
  ```

* `id`  is a unique identifier for your script. It is a randomly generated GUID.
* `name` is the name of your script as it will appear in the [Wix Dev Center](https://dev.wix.com/apps/my-apps). It can only contain letters and the hyphen (-) character. A descriptive name will help you identify your embedded script in your Dev Center extensions page.
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
 
## params.dev.json

This file specifies dynamic parameter values to use during development.

In production, dynamic parameter values are set after installation by calling the Embed Script endpoint as explained in [Preparing your app for production](./embedded-script-task.md#step-2--prepare-your-app-for-production).

This file includes an object containing key-value pairs for each of your dynamic parameters.

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
