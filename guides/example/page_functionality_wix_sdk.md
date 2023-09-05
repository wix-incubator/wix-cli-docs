# Wix SDK

In the Business Buddy app, on the **Products** page, users can choose a product from the site's Wix Store.

To work with the Wix apps on a user's site, we use the [Wix SDK](https://dev.wix.com/api/sdk/sdk-setup:-wix-apps/set-up-the-wix-sdk).

## Permissions

Before getting started making calls to the SDK, you need to request the proper [permissions](../framework/working_with_wix_apis.md#api-permissions).

You can find out which permissions your app needs by checking the API reference for the functions your call. Our app queries store products, so it needs the **Read Products** permission.

You add permissions to your app in the [Developers Center](https://dev.wix.com/) by selecting your app and then going to the **Permissions** tab.

## Get products

After setting up permissions, our app can get a list of products from the site's store.

We start by importing and installing the following:

```tsx
import { createClient } from '@wix/sdk';
import { authStrategy } from '@wix/dashboard-sdk';
import { products } from '@wix/stores';
import { useQuery } from 'react-query';
```

These imports give us access to the Wix SDK, a way to authenticate, and the particular functionality we need from the SDK.

---

Once imported, we create an SDK client that we'll use to make calls to the SDK functions. When creating a client, we use the Dashboard SDK `authStrategy()` function to authenticate our client and we also specify which modules we will use with the client. In our case, we only need the `products` module to access store products.

```tsx
const wixClient = createClient({
  auth: authStrategy(),
  modules: {
    products,
  },
});
```

---

With our client set up, we can use it to query products from the site's Wix Store using the [`queryProducts()`](https://dev.wix.com/api/sdk/stores/products/queryproducts) function and handle any errors that might occur.

Here, we query for products whose names start with whatever has been entered into our `AutoComplete` component.

```tsx
const {
  data: products,
  isLoading,
  error,
} = useQuery(['products', searchQuery], () =>
  wixClient.products.queryProducts().startsWith('name', searchQuery).find()
);

if (error) return <div>Something went wrong</div>;
```

---

While we're at it, let's not forget to replace our dummy current product with the proper type now that we have it.

```tsx
const [currentProduct, setCurrentProduct] = React.useState<
  products.Product | undefined
>();
```

## Populate products

Now that we have a list of products, we can use them to populate the `AutoComplete` component and define what happens when a user selects a product.

First, we set the `AutoComplete` component's `status`.

```tsx
status={isLoading ? 'loading' : undefined}
```

---

Then, we set the `AutoComplete` component's `options` to the products we got from the query by mapping them to a list of options objects with `id` and `value` properties.

```tsx
options={products?.items.map((product) => ({
  id: product._id!,
  value: product.name,
}))}
```

---

Next, we set the `AutoComplete` component's `onSelect` to set the component's current product.

```tsx
onSelect={(e) => {
  setCurrentProduct(
    products!.items.find(
      (product) => product._id === (e.id as string)
    )
  );
}}
```

---

While we're at it, we also set the `AutoComplete` component's `onChange` to set the search query and clear the current product.

```tsx
onChange={(e) => {
  setSearchQuery(e.target.value);
  setCurrentProduct(undefined);
}}
```

---

Finally, we set the `AutoComplete` component's `value` to the current product if there is one.

```tsx
value={currentProduct?.name ?? undefined}
```

## Up next

Now that you understand how to work with the Wix SDK, let's see how to work with the Dashboard SDK.
