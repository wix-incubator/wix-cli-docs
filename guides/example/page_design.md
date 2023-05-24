# Page Design

In the Business Buddy app, the **Products** page looks like this.

![Business Buddy Product Chat](../images/tutorial_chat.png)

We create a design like this, one that is consistent with the rest of the pages in the dashboard, using the React components provided by the [Wix Design System](https://www.wixdesignsystem.com/).

## Products page

In the Business Buddy app, the **Products** page is built from:

- Two main components
- One wrapper component for initializing React context providers
- Some CSS

The **Products** page UI is defined as follows in the **dashboard** folder:

- **/pages/products/page.tsx**: The header and the top cell that it used for selecting the product to chat about
- **/pages/products/ProductChat.tsx**: The bottom cell that displays the selected product and the chat
- **/pages/products/ProductChat.module.css**: CSS for the ProductChat component
- **/withProviders.tsx**: `QueryClientProvider` and `WixStyleReactProvider` wrapper

Let's take a look at the code used to build the page's UI. We'll take a look at the code for the page's functionality a bit later.

### page.tsx

Notice how the code uses components from the Wix Design System to build the page. Other than that, it's pretty much just a standard React component.

> **Note:** At this point, we're showing the code with a hardcoded, dummy product object just to demonstrate how the page will look once a product is selected. We'll see how to get a real product in there a bit later.

```tsx
import {
  AutoComplete,
  Card,
  Cell,
  Divider,
  Layout,
  Page,
} from '@wix/design-system';
import '@wix/design-system/styles.global.css';
import React from 'react';
import { withProviders } from '../../withProviders';
import { ProductChat } from './ProductChat';
import './styles.global.css';

export default withProviders(function ProductsPage() {
  const [currentProduct, setCurrentProduct] = React.useState({
    name: 'Test Name',
    sku: 'Test SKU',
  });
  const [searchQuery, setSearchQuery] = React.useState('');

  return (
    <Page>
      <Page.Header title='Chat about Products' />
      <Page.Content>
        <Layout>
          <Cell>
            <Card>
              <Card.Header title='Select a product to chat about' />
              <Card.Content>
                <AutoComplete
                  placeholder='Select product to chat about'
                  size='large'
                />
              </Card.Content>
            </Card>
          </Cell>

          <Divider />
          <Cell>
            {currentProduct && <ProductChat product={currentProduct} />}
          </Cell>
        </Layout>
      </Page.Content>
    </Page>
  );
});
```

### ProductChat.tsx

Here again, the code uses Wix Design System components to build the component UI.

```tsx
import { Text, Box, Card, Input, Loader } from '@wix/design-system';
import { products } from '@wix/stores';
import React from 'react';
import * as Icons from '@wix/wix-ui-icons-common';
import styles from './ProductChat.module.css';

type Message = {
  author: 'Business Buddy' | 'User';
  text: string;
};

export function ProductChat(props: { product: products.Product }) {
  const [isWaitingForBusinessBuddy, setIsWaitingForBusinessBuddy] =
    React.useState(false);
  const [messageDraft, setMessageDraft] = React.useState<string | undefined>(
    undefined
  );
  const [chatMessages, setChatMessages] = React.useState([] as Message[]);

  return (
    <Card>
      <Card.Header
        title={`Ask Business Buddy about "${props.product.name}"`}
        subtitle={`SKU: ${props.product.sku}`}
      />
      <Card.Content>
        <Box width={'100%'}>
          <Input
            disabled={isWaitingForBusinessBuddy}
            className={styles.userInput}
            suffix={
              <Input.IconAffix>
                <Icons.Send />
              </Input.IconAffix>
            }
            placeholder='Ask Business Buddy something...'
            onChange={(e) => {
              setMessageDraft(e.target.value);
            }}
            value={messageDraft}
          />
        </Box>
        {chatMessages.map((message) => (
          <Box>
            <Text tabName='p'>
              <b>{message.author}</b>: {message.text}
            </Text>
          </Box>
        ))}
        {isWaitingForBusinessBuddy && (
          <Box align='center' padding='8px 0px'>
            <Loader size='small' />
          </Box>
        )}
      </Card.Content>
    </Card>
  );
}
```

### ProductChat.module.css

You can also use CSS to style the elements in a component.

This ProductChat component uses the following CSS to define the width of the input element.

```css
.userInput {
  width: 100%;
}
```

### withProviders.tsx

Finally, here is the code for the `QueryClientProvider` wrapper.

```tsx
import React from 'react';
import { QueryClient, QueryClientProvider } from 'react-query';
import { WixStyleReactProvider } from '@wix/design-system';

export function withProviders(Component: React.ComponentType) {
  return function () {
    return (
      <WixStyleReactProvider>
        <QueryClientProvider client={new QueryClient()}>
          <Component />
        </QueryClientProvider>
      </WixStyleReactProvider>
    );
  };
}
```

## Up next

Now that you understand how dashboard pages are designed, we can now see how to add functionality to it.
