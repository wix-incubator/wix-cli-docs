# App Versions and Deployment

Before deploying an app, you need to create an app version.

When you create an app version, the CLI uploads all your app's build assets to the Wix Apps CDN and registers the app version in [Wix Developers Center](https://dev.wix.com/).

Once you have an app version, you can submit it for review and publish it to the [Wix App Market](https://www.wix.com/app-market) or have users install it on their sites with a direct install URL.

## The Wix Apps CDN

The Wix Apps CDN is a content delivery network that hosts your app's build assets.

Each app in the CDN has its own isolated subdomain. The subdomain has the following format:

```
https://<your-app-id>.wix.run
```

For example, if your app ID is `12345678-1234-1234-1234-123456789012`, your app's build assets are served from `https://12345678-1234-1234-1234-123456789012.wix.run`.

## Build your app

Before creating an app version, you need to build your app.

To build your app, run one of the following:

#### npm

```bash
npm run build
```

#### yarn

```bash
yarn build
```

## Preview versions

Before you create an app version that is registered in Wix Developers Center, you can use preview versions to test your changes and share them with other stakeholders.

Preview versions are not registered in Developers Center and don't affect the users of your production app if you have deployed one.

To create a preview version, run one of the following:

#### npm

```bash
npm run preview
```

#### yarn

```bash
yarn preview
```

This will create and display preview URLs for the various components in your app. You can send these URLs to team members who are [collaborators](https://support.wix.com/en/article/inviting-people-to-contribute-to-your-site) on your development site so they can preview and test changes. You can also use the preview URLs to test your app using browser testing tools like [Puppeteer](https://pptr.dev/), [Selenium](https://saucelabs.com/selenium-getting-started), or [Playwright](https://playwright.dev/).

## Deployable version

After building and testing your app, you can create a new app version and upload your build assets to the Wix Apps CDN.

To create a new app version, run one of the following:

#### npm

```bash
npm run create-version
```

#### yarn

```bash
yarn create-version
```

This guides you through the process of creating a new app version. The CLI prompts you to choose a version type for your app version (major or minor).

### Version types

Apps installed on Wix sites use Semantic Versioning (SemVer), where each version is either a major or minor version. SemVer is used to determine if new app versions can be automatically updated on sites that have the app already installed or if the site admin needs to take some action to install the updated version.

Choose the version type carefully when creating a new app version. Consider the changes you have introduced in the new version.

- **Major**: For versions that include breaking changes
- **Minor**: For versions that are backward compatible

Minor versions are updated automatically on sites that have your app installed. Major versions are not updated automatically, since they contain breaking changes. To update to a major version, site admins need to manually update their site's app version to the new major version from the **Manage Apps** page in the site's dashboard.

For example, if an admin installed your app while the current version was `1.0`, the site automatically updates when you deploy versions `1.1`, `1.2`, and so on. However, when you release version `2.0`, the site will not automatically update.

## Manage app versions

You can view and manage your app versions in the **Versions** tab of Wix Developers Center. Use this tab to submit app versions to the [Wix App Market](https://www.wix.com/app-market).
