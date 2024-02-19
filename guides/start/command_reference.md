---
type: reference
---

# Wix CLI Commands for Apps

This article documents the CLI commands for building apps on the [Wix Platform](https://dev.wix.com/docs/build-apps/developer-tools/cli/get-started/platform-overview).

For a detailed explanation of the process and how to initially create an app, see our [Quick Start article](https://dev.wix.com/docs/build-apps/developer-tools/cli/get-started/quick-start).

## Command overview

| Category          | Command            | Description                                                  |
|----------------|--------------------|--------------------------------------------------------------|
| CLI for Apps   | [dev](#dev)        | Starts a local development environment that serves your app. |
|                | [build](#build)    | Builds your app.                                             |
|                | [serve](#serve)    | Serves your built app.                                       |
|                | [preview](#preview)| Creates an online preview of your built app.                 |
|                | [create-version](#create-version)   | Starts a process that guides you through creating a new app version in the [Wix Developers Center](https://dev.wix.com/apps/my-apps). |
| Global CLI     | [login](#login)    | Logs in to your Wix account.                                   |
|                | [logout](#logout)  | Logs out of your Wix account.                                  |
|                | [telemetry](#telemetry)| Opts in or out of sending anonymous usage information (telemetry) to Wix. |
|                | [whoami](#whoami)  | Displays the email address of the logged-in Wix user.         |


## dev

```tsx
wix app dev
```

Starts a local development environment that serves your app. Follow the prompts to open the generated dashboard page in your browser.

The development environment is set up for hot reloading, so any changes you make to your code are immediately reflected in the browser.

### dev flags

| Flag           | Alias     | Description                             |
| -------------- | --------- | --------------------------------------- |
| `--https`  | `-s` | Starts your local development server on HTTPS. Use this flag if your browser does not allow iframes that contain HTTP pages within HTTPS pages. |

## build

```tsx
wix app build
```

Builds your app.

You must build your app before:

* Creating a version
* Previewing your app
* Serving your app

## serve

```tsx
wix app serve
```

Serves your app.

Serving creates a local preview of your built app. This preview is accessible only locally and can’t be shared with others.

## preview

```tsx
wix app preview
```

Creates an online preview of your built app and provides an inline link to an app preview URL in the console.

An app preview URL is a URL you can share with team members who are [collaborators](https://support.wix.com/en/article/inviting-people-to-contribute-to-your-site) on your development site. It directs to a dashboard page where they can preview and test your app.

By default, the code for your app preview is hosted on Wix's servers.

### preview flags

| Flag                 | Alias         | Description                                                           |
| -------------------- | ------------- | --------------------------------------------------------------------- |
| `--site <site-id>`   | `-s <site-id>` | Allows you to set the `site` used for application preview URLs. If this flag is not provided, the `site-id` defaults to the currently-selected development site.<br><br>You can get the ID of a site from the site URL in your browser. For example, the site ID appears after the `/dashboard/` part of this URL:<br>![Site ID](site-id.png) |
| `--base-url <url>`     |   | Sets the base URL for uploading your static files to an external CDN. If this flag is not provided, the code for your preview will be hosted on Wix's servers.

## create-version

```tsx
wix app create-version
```

Starts a process that guides you through creating a new app version in the [Wix Developers Center](https://dev.wix.com/apps/my-apps).

Once the app version is created, you can [submit it for review](https://devforum.wix.com/kb/en/article/submit-your-app-for-review) and publish it to the [Wix App Market](https://www.wix.com/app-market).

To learn more about app versions, see [App Versions and Deployment](../workflow/app_versions_and_deployment.md).

### create-version flags

| Flag                 | Alias         | Description                                                           |
| -------------------- | ------------- | --------------------------------------------------------------------- |
| `--version-type <type>`   | `-t <type>` | Specifies the type of version to create. <br><br> Accepted values for `<type>`: `major`, `minor`
| `--approve-preview`   | `-y` | Automatically approves continuing with the publishing process after the preview is provided.
| `--base-url <url>`     |   | Sets the base URL for uploading your static files to an external CDN. If this flag is not provided, the code for your preview will be hosted on Wix's servers.

## login

```tsx
wix login
```

Logs in to your Wix account.

## logout

```tsx
wix logout
```

Logs out of your Wix account.

## telemetry

```tsx
wix telemetry <state>
```

Opts in or out of sending anonymous usage information (telemetry) to Wix.

Accepted values for `<state>`: `on`, `off`

## whoami

```tsx
wix whoami
```

Displays the email address of the logged-in Wix user.
