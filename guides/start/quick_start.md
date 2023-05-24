# Quick Start Guide

Follow this guide to get started building your first Wix App using the Wix CLI for Apps.

To build a new app, you will:

- Create a new app project
- Run the app locally
- Add new Dashboard Pages & configure the Dashboard Sidebar
- Interact with the Dashboard using the Dashboard SDK
- Create app versions in the Wix Developers Center

> This quick start guide is intended for developers building new Wix Apps. If you already have an existing app and want to develop new features for it using the CLI, see [Integrate with Existing Apps](../workflow/integrate_with_existing_apps.md).

## Prerequisites

Before getting started, make sure you have the following set up:

- [Node.js](https://nodejs.org/en/) (v16.20.0 or higher)
- `npm` or `yarn`
- macOS, Linux, or Windows
- A Wix Developer Account - [Sign up here](https://users.wix.com/signin?loginDialogContext=signup&referralInfo=HEADER&postLogin=https:%2F%2Fdev.wix.com%2Fdc3%2Fmy-apps&postSignUp=https:%2F%2Fdev.wix.com%2Fdc3%2Fmy-apps&forceRender=true)

## Create a new app project

Let's start by creating a new app project.

To create a new app project, run one of the following and follow the command prompts:

#### npm

```bash
npm create @wix/app@latest
```

#### yarn

```bash
yarn create @wix/app
```

This registers a new app in the Wix Developers Center and creates a new project in your local file system.

### Choose a development site

During the app creation process, the CLI prompts you to choose a development site. A development site is a site that you use to run and test your app. You can choose an existing site or create a new one.

To learn more about working with a development site, see [Development Sites](../workflow/development_sites.md).

### Project structure

The CLI generates a new app project in your local file system. The project contains all the files your app needs to run locally and in production.

The project includes:

- Initial boilerplate code for a simple app with a dashboard page
- A `package.json` file with your apps dependencies

To learn more about dashboard pages in the project structure, see [Dashboard Pages](../framework/dashboard_pages.md).

## Run the app locally

Now that we've created an app project, we can run it locally to see it in action.

To run the new app project locally, run one of the following:

#### npm

```bash
npm run dev
```

#### yarn

```bash
yarn dev
```

This starts a local development environment that serves your app. Follow the prompts to open the generated dashboard page in your browser. The development environment is set up for hot reloading, so any changes you make to your code will be reflected in the browser.

Let's make a small change in the boilerplate code to see how you edit your project:

1. Open the `src/dashboard/pages/page.tsx` file.
1. Add this `import` to the top of the file.
   ```tsx
   import { showToast } from '@wix/dashboard-sdk';
   ```
1. Replace the current button with this button that includes an `onClick` event handler:
   ```tsx
   <Button
     onClick={() => {
       showToast({
         message: 'A toast for our new Wix App!',
         type: 'success',
       });
     }}
   >
     Toast Button
   </Button>
   ```
1. Save the file and go back to the browser. Notice how the page now has the new button.
1. Click the button. A toast message appears at the top of the dashboard page.

To learn more about working with the Dashboard SDK and its different methods, see the [Dashboard SDK](https://dev.wix.com/api/client/dashboard-sdk/).

## Build the app

Now that we've run our app, we can build it.

To build the app, run one of the following:

#### npm

```bash
npm run build
```

#### yarn

```bash
yarn build
```

## Create an app preview URL

Now that we've made a change to our app and built it, let's create an app preview URL.

An app preview URL is a URL you can share with team members who are [collaborators](https://support.wix.com/en/article/inviting-people-to-contribute-to-your-site) on you development site. The link allows them to preview and test your app.

To create an app preview URL, run one of the following:

#### npm

```bash
npm run preview
```

#### yarn

```bash
yarn preview
```

This displays the app preview URL. Copy the URL and open it in a new browser tab. The URL takes you to the same dashboard page that you saw previously. Use this URL to share your app with others for testing and feedback.

## Create an app version

Now that we have a working app, let's create an app version.

An app version allows you to publish an app to the [Wix App Market](https://www.wix.com/app-market) or install it on a site with a direct install URL.

To create an app version, run one of the following and follow the command prompts:

#### npm

```bash
npm run create-version
```

#### yarn

```bash
yarn create-version
```

This guides you through creating a new app version in the Wix Developers Center. Once the app version is created, you can optionally [submit it for review](https://devforum.wix.com/kb/en/article/submit-your-app-for-review) and publish it to the [Wix App Market](https://www.wix.com/app-market).

To learn more about app versions, see [App Versions and Deployment](../workflow/app_versions_and_deployment.md).

## Next steps

You now have a fully working app that can be installed on Wix sites. Take some time to tinker a bit with the generated code and try to add more features to your app.

Use the following resources to continue building you app:

- [Dashboard Pages](../framework/dashboard_pages.md): Learn how to add new dashboard pages to your app, configure the dashboard sidebar, and more.
- [Dashboard SDK](https://dev.wix.com/api/sdk/dashboard-sdk): Learn how to interact with the Wix Dashboard using the Dashboard SDK.
- [Wix Design System](https://wixdesignsystem.com): Learn how to use the Wix Design System to build beautiful Wix Apps.
