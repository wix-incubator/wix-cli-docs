# Communicate with the Backend

In the Business Buddy app, the **Products** and **Settings** dashboard pages communicate with an app backend to work with the OpenAI API and to store user settings.

In general, you communicate with a backend server from a Wix App the same way you would in any other type of app.

You do, however, need to pay special attention in order to:

- Authenticate that requests to your server are coming from your app and not a malicious user
- Differentiate between requests from the various instances of your app installed on multiple sites

Later, we'll see how to do all that on the server. For now, let's focus on what we need to do on the frontend.

## Instance ID

Each time an app is installed on a site a unique instance ID is assigned. You can use the instance ID to authenticate requests sent from an app and to know which instance of the app is sending the request.

Let's see how we add code to send requests to the server using the instance ID. In our app, we've placed this code in a file named **utils.ts** in the **dashboard** folder.

---

First, we create a function to get the app instance from the app's URL.

```ts
export function getAppInstance() {
  return new URLSearchParams(window.location.search).get('instance')!;
}
```

---

Next, we create a function to fetch data from the server using the app instance to authorize the request. Notice how we call the `getAppInstance()` function described above to create the authorization header.

```ts
export async function fetchWithWixInstance(
  relativePath: string,
  method: string,
  body?: any
) {
  const res = await fetch(
    `${import.meta.env.VITE_API_BASE_URL}/${relativePath}`,
    {
      method,
      headers: {
        Authorization: getAppInstance(),
        ...(body && { 'Content-Type': 'application/json' }),
      },
      body: body && JSON.stringify(body),
    }
  );

  const json = await res.json();
  return json;
}
```

## Fetching Data

Now that we've set up the plumbing to make secure requests to the server, we can use the `ProductChat` component to send and receive chat messages.

---

First, in **ProductChat.tsx** we import the utility function described above.

```tsx
import { fetchWithWixInstance } from '../../utils';
```

---

Then, we add a function to submit messages, send them to the server, and update the state with the response. Notice how this function calls `fetchWithWixInstance()` to post a new message to the server.

```tsx
async function submitMessage() {
  const newMessage: Message = {
    author: 'User',
    text: messageDraft ?? '',
  };
  setChatMessages((state) => [...state, newMessage]);
  setMessageDraft('');
  setIsWaitingForBusinessBuddy(true);
  const { message } = await fetchWithWixInstance(`chat/product`, 'POST', {
    messages: [...chatMessages, newMessage],
    product: JSON.stringify(props.product, null, 2),
  });
  setChatMessages((state) => [
    ...state,
    {
      author: 'Business Buddy',
      text: message,
    },
  ]);
  setIsWaitingForBusinessBuddy(false);
}
```

---

Finally, we call `submitMessage()` when the send icon is clicked.

```tsx
<Icons.Send onClick={submitMessage} />
```

And we call `submitMessage()` again when enter is pressed in the `Input` component.

```tsx
onEnterPressed = { submitMessage };
```

## Up next

Now that you understand how to communicate with your app's backend, we can now see how to deal with those communications on the server.
