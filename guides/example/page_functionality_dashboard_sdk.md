# Dashboard SDK

In the Business Buddy app, when a user changes the behavior setting on the **Settings** page, a toast is shown at the top of the dashboard page.

To show toasts, update dashboard components based on state changes, and open and close dashboard modals, we use the [Wix Dashboard React SDK](TODO: ink here).

## Show a toast

First we need to import the [`useDashboard()`](TODO: link here) hook from the Dashboard React SDK.

```tsx
import { useDashboard } from '@wix/dashboard-react';
```

---

Then, we need to call the `useDashboard` hook in whichever component we need to access dashboard APIs from. In our settings page component code we'll add the following:

```tsx
const { showToast } = useDashboard();
```

Then, all we need to do is call the `showToast()` function and provide a message and type. In our app, we want to show the toast when a behavior setting has been successfully changed, so we call `showToast()` in the `onSuccess` of the mutation used by the **Save** button.

```tsx
{
  onSuccess: () => {
    showToast({
      message: "Changes saved!",
      type: "success",
    });
  },
}
```

## Up next

Now that you understand how to work with the Dashboard SDK, let's see how to send requests to an app backend.
