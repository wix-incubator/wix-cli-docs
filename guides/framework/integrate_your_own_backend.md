# Integrate Your Own Backend

When building an app, you often need to create a backend so you can:

- Use existing APIs that you developed
- Use third-party APIs
- Work with a database

In order to do these things in a secure manner, you need to create your own backend. You can use any server technology you want to create your backend.

There are a few things you need to keep in mind when implementing your backend:

- **CORS**: You need to set up CORS properly to make sure your server accepts requests from your app
- **Authentication**: You need to check that requests to your server are coming from your app and not a malicious user
- **Multiple instances**: You can optionally differentiate between the multiple instances of your app when they make requests to the server

## CORS

When making HTTP requests from your app code to your backend, you need to make sure that your backend allows those requests.

Your app frontend code is hosted on the [The Wix App CDN](../workflow/app_versions_and_deployment.md#the-wix-apps-cdn). The app is served from its own `wix.run` subdomain.

To allow your app frontend code to make HTTP requests to your backend, you need to allow CORS requests from your frontend's domain.

Add the following headers to your backend's HTTP responses to all CORS requests from your app.

```http
Access-Control-Allow-Origin: https://<your-app-id>.wix.run
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
```

Replace **\<your-app-id\>** with your app's ID, which can be found in your app's dashboard in the [Wix Developers Center](https://dev.wix.com/apps/). Also, only allow the HTTP methods that your app requires.

For local development, you also need to allow CORS requests from `localhost:5173` (or preferably, from all origins with `localhost`).

```http
Access-Control-Allow-Origin: http://localhost:5173
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
```

## Authenticate incoming requests

Once you expose a backend API to make it available for your app, you need to make sure that requests to your API are indeed coming from your app and not from some malicious user.

To authenticate requests, you need to use your app's unique app instance object, which is signed with your app's secret key.

### App instance object

An app instance object is a JSON object that contains information about the site an app is installed on, the current user, and the current instance of your app. The app instance object is passed to your app's iframe as a query parameter.

The app instance object contains the following useful fields:

- `instanceId`: ID of the current instance of your app
- `uid`: ID of the user who is logged into the site your app is installed on

To learn more about the app instance and its fields, see [App Instance](https://devforum.wix.com/kb/en/article/app-instance-client-side).

### Get the app instance

In your app frontend, you need to retrieve the app instance signed string so you can send it along with your request to the backend.

To retrieve the app instance signed string, use the following helper function in your code:

```ts
export function getAppInstance() {
  return new URLSearchParams(window.location.search).get('instance')!;
}
```

### Send the app instance

Once you've retrieved the app instance, you need to send it in requests you make to your backend. The backend uses the app instance to authenticate your request and extract any information needed from the app instance.

To make HTTP requests to your backend with the signed instance, use something similar to the following helper function:

```ts
export async function fetchWithWixInstance(url: string, options: RequestInit) {
  return fetch(url, {
    ...options,
    headers: {
      Authorization: getAppInstance(),
      ...options.headers,
    },
  });
}
```

This function sends the app instance in the authorization header when making requests to the backend.

### Validate the app instance

When a request is made to your app backend, you should authenticate it before continuing to process it.

You need your app's secret key to authenticate requests. You can get your app's secret key from the **OAuth** page in your app's dashboard in the [Wix Developers Center](https://dev.wix.com/apps).

> **Important security note:** Be sure to store your app secret securely on your server.

To authenticate a request, extract the signature from the app instance sent in the request and verify it was signed with your app's secret key.

For parsing examples in a number of languages, see [App Instance](https://devforum.wix.com/kb/en/article/app-instance-client-side#parsing-examples).

Here is an example of how to parse the instance in TypeScript:

```ts
import { createHmac } from 'crypto';

export function parseInstance(
  instance: string,
  appSecret: string
): {
  instanceId: string;
  appDefId: string;
  signDate: string;
  uid: string;
  permissions: 'OWNER';
  demoMode: boolean;
  siteOwnerId: string;
  siteMemberId: string;
  expirationDate: string;
  loginAccountId: string;
  pai: null;
  lpai: null;
} | null {
  var parts = instance.split('.'),
    hash = parts[0],
    payload = parts[1];

  if (!payload) {
    return null;
  }

  if (!validateInstance(hash, payload, appSecret)) {
    return null;
  }

  return JSON.parse(base64Decode(payload, 'utf8'));
}

function validateInstance(hash: string, payload: string, secret: string) {
  if (!hash) {
    return false;
  }

  hash = base64Decode(hash);

  var signedHash = createHmac('sha256', secret)
    .update(payload)
    .digest('base64');

  return hash === signedHash;
}

function base64Decode(input: string, encoding: 'base64' | 'utf8' = 'base64') {
  return Buffer.from(
    input.replace(/-/g, '+').replace(/_/g, '/'),
    'base64'
  ).toString(encoding);
}
```

## Call Wix APIs

You can call Wix APIs from your backend using the [Wix REST API](https://dev.wix.com/docs/rest). You will need to authenticate your requests using either [OAuth](https://dev.wix.com/api/rest/getting-started/authentication) or [API keys](https://dev.wix.com/api/rest/getting-started/api-keys).
