# Receive Backend Requests

In the Business Buddy app, the backend server receives requests for chat messages and for storing and retrieving settings values. Each of these operations is performed by calling an endpoint exposed on the server.

> In this implementation we use a simple [Express](https://expressjs.com/) server. You're free to use any type of server for your app's backend.

When an app server receives a request to an endpoint, you usually need to:

- Authenticate that the request is from your app and not a malicious user
- Know which instance of your app has sent the request

You'll also need to do some CORS setup to make sure your server allows the requests from your app.

## CORS

To set up CORS properly, we need to add headers that allow access from certain origins and with certain HTTP methods.

In our case, we whitelist **localhost** for testing purposes and the URL for our app frontend on the Wix App CDN.

The URL of an app on the CDN is in the following format:

```
https://<your-app-id>.wix.run
```

And this is how we whitelist the URLs on our server.

```ts
app.use(
  cors({
    origin(requestOrigin, callback) {
      const whitelist = [
        'http://localhost',
        'https://1522de8b-ea83-423b-9da0-166fdc7ef372.wix.run',
      ];

      if (
        requestOrigin &&
        whitelist.some((whitelistOrigin) =>
          requestOrigin.includes(whitelistOrigin)
        )
      ) {
        callback(null, true);
      } else {
        callback(new Error('Not allowed by CORS'));
      }
    },
  })
);
```

## Authentication

Now that we've dealt with CORS issues, the next thing to do is to authenticate the requests coming into our server.

To authenticate requests, we first need our app's secret key from the Wix Developers Center. We get the key from the **OAuth** page in our app's dashboard. Then we add the key to an environment variable named `WIX_APP_SECRET`.

Now that we have the key, we can write code to authorize the request. In our case, we write this code in a file named **utils.ts**.

---

First, we create the `authorizeWixRequest()` function. This function reads the authorization header sent from our app frontend and checks it using the `parseInstance()` function described below.

```ts
export function authorizeWixRequest(req: express.Request) {
  const authorization = req.headers.authorization;
  if (!authorization) throw new Error('No authorization header');
  const instance = parseInstance(
    authorization,
    process.env.WIX_APP_SECRET as string
  );
  if (!instance) throw new Error('Invalid instance');
  return instance;
}
```

---

Next, we create the `parseInstance()` function to parse the received app instance and check it against our app's secret key using the `validateInstance()` function described below.

```ts
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
    console.log('Provided instance is invalid: ' + instance);
    return null;
  }

  return JSON.parse(base64Decode(payload, 'utf8'));
}
```

---

Then, we create the `validateInstance()` function to authenticate the instance against our app secret.

```ts
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
```

---

Finally, to use this authentication code, we simply call the `authorizeWixRequest()` function and pass it an incoming request. For example, when we receive a request for a product chat we authorize the request before continuing.

```ts
app.post("/chat/product", async function (req, res) {
  const { instanceId } = authorizeWixRequest(req);

  // continue to process request...
}
```

## Multiple Instances

In the Business Buddy app, the backend server stores settings per app instance. We use the instance ID to associate a particular setting value with a particular instance of our app.

In our app, we do this by storing the instance ID alongside each setting value in a simple key-value store.

---

You might have noticed that the `authorizeWixRequest()` function mentioned above returns the parsed instance information, including an `instanceId`. We can pass that information to the key-value store when we process a request, such as a `/settings` POST, as you can see here.

```tsx
app.post('/settings', async function (req, res) {
  const { instanceId } = authorizeWixRequest(req);
  const behaviorDirective = req.body.behaviorDirective;
  saveBehaviorDirective(instanceId, behaviorDirective);
  res.sendStatus(200);
});
```

---

Then, in the key-value store, we can store the value of the `instanceId` as the key and the setting text as the value.

```tsx
let behaviorDirectives: { [instanceId: string]: string } = {};

export function saveBehaviorDirective(instanceId: string, directive: string) {
  behaviorDirectives[instanceId] = directive;
}
```

## Up next

Now that you understand how to receive backend request from an app, you've got all the basics you need to know. At this point, feel free to play around with the example app some more, or get started on writing your own awesome app.
