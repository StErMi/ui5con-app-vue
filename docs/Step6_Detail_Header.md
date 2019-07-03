# Detail Page Header

It is now time to add a Header component for our Detail Page.
The Header component should look like the following:

![Details Header](./images/details-header.png?raw=true "Details Header")


You should now create a file called `Header.vue` component inside `src/views`.

An empty `Header` component should looks like this:

```js
<template>
  <h1>Hello World</h1>
</template>

<script>
export default {
  name: "header",
  data: function() {
    return {};
  }
};
</script>
```

To place this component inside the `Detail` component, add a <Header /> tag to the Detail's `<template></template>` tag.

Don't forget to import the Header either in the Detail.vue and add it to the list of components:
```js
import Header from "@/views/Header.vue"
```



e.g.

```html
<template>
  <div class="detail-page">

    <Header />

    <main class="detail-page-content">
      <ui5-table>
      
      ...
```

You should now be able to see the hello world heading above the table.
Next we will add the real building blocks of the Header component.

import `Tab` and `TabContainer` inside `App.vue`
```js
import "@ui5/webcomponents/dist/Tab";
import "@ui5/webcomponents/dist/TabContainer";
```

It contains
- a section that wraps a `ui5-title` and a `ui5-button`
- another section that represents a `ui5-tabcontainer` with 4 `ui5-tab`s inside

```html
<template>
  <header class="detail-page-header">
    <div class="detail-page-header-bar">
      <ui5-title>Inventory</ui5-title>

      <ui5-button design="Transparent" icon="sap-icon://action" class="action-button"></ui5-button>
    </div>

    <ui5-tabcontainer class="detail-page-header-menu" fixed collapsed>
      <ui5-tab text="All Items" additional-text="42"></ui5-tab>
      <ui5-tab text="Non-Perishable" additional-text="42"></ui5-tab>
      <ui5-tab text="Perishable" additional-text="42"></ui5-tab>
      <ui5-tab text="Alerts" additional-text="42"></ui5-tab>
    </ui5-tabcontainer>
  </header>
</template>
```

Lets add some styles to the `Header` in order to have a better placement for the components:

```css
<style scoped>
.detail-page-header-bar {
  height: 6rem;
  padding: 1rem 3rem;
  display: flex;
  justify-content: space-between;
  box-sizing: border-box;
  background: var(--sapUiGroupContentBackground, #ffffff);
}

.detail-page-header-menu {
  border: 1px solid var(--sapUiListTableGroupHeaderBorderColor, #d9d9d9);
}
</style>
```
![Details Header Styled](./images/header-before-events.png?raw=true "Details Header Styled")

We should now fire some events when a tab is clicked in order to make our Detail page filter the table.
Furthermore, we should pass some properties such as products, non perishable products count, etc. and visualize them in the tab container. 
Lets pass the properties to the Header from the Details `template` render function.

```html
<Header
    :products="products"
    :nonPerishableCount="filterNoPerishableProducts(products).length"
    :perishableCount="filterPerishableProducts(products).length"
    :alertCount="filterAlertProducts(products).length"
    @tabPress="applyFilter"
/>
```

You also need to have methods that get the count of items based on the filter Type. e.g.

```js
filterPerishableProducts(items) {
	return items.filter(product => product.perishable);
},

filterNoPerishableProducts(items) {
	return items.filter(product => !product.perishable);
},

filterAlertProducts(items) {
	return items.filter(product => (product.status === "Deterioating" || product.status === "Re-Stock"));
}
```

We should also add a listener to the pressed tab. It will receive a filter type e.g. "perishable" and update the state of the Detail page.

```js
applyFilter(filterType) {
    const products = this.filterItems(filterType, this.products);
    this.filteredProducts = products;
    this.filterType = filterType;
}

filterItems(filterType, items) {
    let filteredProducts = [];

    switch (filterType) {
        case "all":
            filteredProducts = items;
            break;
        case "noPerishable":
            filteredProducts = this.filterNoPerishableProducts(items);
            break;
        case "perishable":
            filteredProducts = this.filterPerishableProducts(items);
            break;
        case "alerts":
            filteredProducts = this.filterAlertProducts(items);
            break;
        default:
            filteredProducts = items;
            break;
    }

    return filteredProducts;
}
```

We now pass all the correct data to the `Header`. We should now implement what it will display / do, based on the passed properties.

Lets first bind the properties to the `tab`s `additional-text` property.

Now we need to expose and use the new properties required by the `Header` component like this

```js
<script>
export default {
  name: "detail-header",
  props: {
    products: Array,
    nonPerishableCount: Number,
    perishableCount: Number,
    alertCount: Number
  },
  data: function() {
    return {};
  }
};
</script>
```

You can read more about Vue.js Component props [here](https://vuejs.org/v2/guide/components-props.html)

```html
<ui5-tabcontainer class="detail-page-header-menu" fixed collapsed>
    <ui5-tab data-filter-type="all" text="All Items" :additional-text="products.length"></ui5-tab>
    <ui5-tab data-filter-type="noPerishable" text="Non-Perishable" :additional-text="nonPerishableCount"></ui5-tab>
    <ui5-tab data-filter-type="perishable" text="Perishable" :additional-text="perishableCount"></ui5-tab>
    <ui5-tab data-filter-type="alerts" text="Alerts" :additional-text="alertCount"></ui5-tab>
</ui5-tabcontainer>
```

Once we have our properties bound to the tabs, we should add the interaction with them.

We've defined that `tabPress` will be fired once the user interacts with a tab. It should pass a parameter to the listener - type of the filter. In order to implement such functionality, we need to know which tab is pressed in the `ui5-tabcontainer`'s `itemSelect` event.

In order to pass the event filter type, we should somehow mark our `ui5-tab`s with information what they are filtering on. As this is just a dom, we can add a custom attribute e.g. `data-filter-type` to our tabs.

```html
<ui5-tab data-filter-type="all" text="All Items" :additional-text="products.length"></ui5-tab>
<ui5-tab data-filter-type="noPerishable" text="Non-Perishable" :additional-text="nonPerishableCount"></ui5-tab>
<ui5-tab data-filter-type="perishable" text="Perishable" :additional-text="perishableCount"></ui5-tab>
<ui5-tab data-filter-type="alerts" text="Alerts" :additional-text="alertCount"></ui5-tab>
```

We can now easily identify which tab corresponds to which filter. Now we just need to listen to the `itemSelect` event of the `ui5-tabcontainer` and emit to the listener an event with the selected `filter-type` parameter like this:

```html
<ui5-tabcontainer class="detail-page-header-menu" fixed collapsed @itemSelect="$emit('tabPress', $event.detail.item.getAttribute('data-filter-type'))">
```

![Step 6 result](./images/step6-result.png?raw=true "Step 6 result")


### [Step #7 - Detail Filter Bar](./Step7_Detail_FilterBar.md)
