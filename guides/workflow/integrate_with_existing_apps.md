# Integrate with Existing Apps

You can use the Wix CLI for Apps to work with existing apps that are already registered in the Wix Developers Center. This allows you to add new features to your app using the CLI without having to migrate the entire app to your CLI project.

Because the Wix CLI for Apps is a new tool, it does not yet support all Wix App Platform features. By working with both the Developers Center and the CLI you can continue to create and manage existing app extensionss in the Developers Center, while still using the CLI for its current capabilities.

## Extend an existing app

To extend an existing app with a new Wix CLI for Apps project:

1. Choose **Extend existing application** when [creating a new project](../start/quick_start.md#create-a-new-app-project)
1. Choose the existing app to extend

When working on the CLI extension project locally (or in preview versions), you will only be able to work with the app extensions that are registered in the Wix CLI project. The app extensions from the Developer Center will still be loaded into your development site, but they will use their production versions.

## Release a new version

When you are ready to release a new version of your extended app, use the CLI to [create a new version and upload it to the CDN](./app_versions_and_deployment.md).

It's important to note that when you create a new app version from the CLI, it also includes any changes that you made in other app extensions that are not registered in the Wix CLI project. Take this into account when creating a new app version and ensure that your other app extensions are ready to be released.
