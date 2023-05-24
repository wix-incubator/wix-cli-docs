# Development Sites

Wix Apps are not standalone web apps. They need to be installed on a Wix site to run. To test your app during the development process, you can use a development site.

As you work locally with the CLI, you can use the CLI to open the different parts of your app on a development site in your browser.

## Premium development sites

You can use any site in your Wix account as your development site. However, your app might have features that can only be used on premium sites. To allow you to test your app on premium sites, you can create up to 5 premium development sites for free.

## Associate a development site with an app

When you [create a new app project](../start/quick_start.md#create-a-new-app-project) using the CLI, you'll be prompted to choose a development site for the app. You can choose an existing site or create a new one. Follow the prompts in the CLI to install your app on the selected site.

## Change an app's development site

Throughout the development process it's a good idea to use multiple development sites to test your app in different scenarios. You can change the development site associated with an app at any time.

To change an app's development site:

1. Go to the `wix-dev` menu

   #### npm

   ```bash
   npm run dev
   ```

   #### yarn

   ```bash
   yarn dev
   ```

2. Use the `D` hotkey to assign a new development site

## Development site isolation

By default, development sites are not shared between developers of the same app. They are used for the local work of each individual developer. This isolation from other developer's work is helpful when testing your code.

Because development sites are unique to each developer, information about your current development site is stored in a git-ignored file in your project.
