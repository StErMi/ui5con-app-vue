# Page Overview

Lets now have a look at the Detail page.
We will try to build a page following some guidelines mentioned in [List Report Floorplan](https://experience.sap.com/fiori-design-web/list-report-floorplan-sap-fiori-element/).

![Details Header](./images/details.png?raw=true "Detail Header")

As React is recommending to split your components for an easier maintainability, we will create 2 more components - `Header` and `FilterBar` which represent 2 of the main building blocks of the Detail page.

![Details Header Splitted](./images/details-splitted.png?raw=true "Detail Header Splitted")


We will be working with a data set called `product.json`, placed inside a `src/data/` directory.


In order to have all resources available, copy [these files](https://github.com/stermi/ui5con-app-vue/tree/master/public/img) to your `/src/assets/img` folder.


The `Detail` page will have a global state which holds the following information:
-  `products`: - all of the items listed in `products.json` (we will query this set and set the result to the `filteredProducts` )
- `filteredProducts`: currently visibile items in the table (this will be changed a lot based on the user interactions)
- `filterType`: the type of the filter that is currently applied to the page

Lets now define a state to our component and add a file for styling it (Detail.vue).

```js
<script>
import products from "../data/products.json";

// These are the web components that we will be using here
import "@ui5/webcomponents/dist/Title";
import "@ui5/webcomponents/dist/Table";
import "@ui5/webcomponents/dist/TableColumn";
import "@ui5/webcomponents/dist/TableRow";
import "@ui5/webcomponents/dist/TableCell";
import "@ui5/webcomponents/dist/Badge";
import "@ui5/webcomponents/dist/Dialog";
import "@ui5/webcomponents/dist/Popover";
import "@ui5/webcomponents/dist/Select";
import "@ui5/webcomponents/dist/DatePicker";
import "@ui5/webcomponents/dist/TextArea";

export default {
  name: "detail",
  components: {},
  data: function() {
    return {
      products: [...products],
      filteredProducts: [...products],
      filterType: "all"
    };
  },
  methods: {
    getProductImage(img) {
      return img.startsWith("http") ? img : require(`@/assets${img}`);
    }
  }
};
</script>
```

Lets now render something on the screen. As we mentioned above, we will have 2 components `Header` and `FilterBar`, separated from the `Detail`.

Add the following template to the Detail.vue.

```html
<template>
  <div class="detail-page">
    <main class="detail-page-content">
      <ui5-table>
        <ui5-table-column slot="columns">
          <ui5-label class="table-column-header-content">Product</ui5-label>
        </ui5-table-column>

        <ui5-table-column slot="columns">
          <ui5-label class="table-column-header-content">Price</ui5-label>
        </ui5-table-column>

        <ui5-table-column slot="columns">
          <ui5-label class="table-column-header-content">Location</ui5-label>
        </ui5-table-column>

        <ui5-table-column slot="columns">
          <ui5-label class="table-column-header-content">
            Order
            date
          </ui5-label>
        </ui5-table-column>

        <ui5-table-column slot="columns">
          <ui5-label class="table-column-header-content">Image</ui5-label>
        </ui5-table-column>

        <ui5-table-column slot="columns">
          <ui5-label class="table-column-header-content">Status</ui5-label>
        </ui5-table-column>

        <ui5-table-row :key="item.key" v-for="item of filteredProducts">
          <ui5-table-cell>
            <ui5-label class="table-cell-content">
              <b>{{item.name}}</b>
            </ui5-label>
          </ui5-table-cell>
          <ui5-table-cell>
            <span class="table-cell-content">{{item.price}}</span>
          </ui5-table-cell>
          <ui5-table-cell>
            <span class="table-cell-content">{{item.location}}</span>
          </ui5-table-cell>
          <ui5-table-cell>
            <span class="table-cell-content">{{item.orderDate}}</span>
          </ui5-table-cell>
          <ui5-table-cell>
            <span class="table-cell-content">
              <img alt class="image-cell" :src="getProductImage(item.img)" />
            </span>
          </ui5-table-cell>
          <ui5-table-cell>
            <span class="table-cell-content">
              <ui5-badge color-scheme="0">{{item.status}}</ui5-badge>
            </span>
          </ui5-table-cell>
        </ui5-table-row>
      </ui5-table>
    </main>
  </div>
</template>
```

We now can see the table displayed in our page. We need to add some styling for the layout of the page to `Detail.vue`. An example of styling would be the following:

```css
<style scoped>
.detail-page-content {
  padding: 3rem;
}

.table-column-header-content,
.table-cell-content {
  display: flex;
  width: 100%;
  height: 100%;
  align-items: center;
}

.image-cell {
  height: 3rem;
  width: 3rem;
}
</style>
```

Last thing we should do for this page is to style a bit the badge of the status column. We can define a method `getBadgeType` which returns a value to be set to the `color-scheme` property of the badge.

Here is an example of the implementation add the `getBadgeType` method to the `methods` section:

```js
getBadgeType(type) {
    switch (type) {
        case "In-Stock":
            return "8";
        case "Deterioating":
            return "2";
        case "Re-Stock":
            return "1";
        default:
            return "0";
    }
}
```

and call this method when binding the `color-scheme` of the badge in the last cell of the row:

```html
<ui5-badge :color-scheme="getBadgeType(item.status)">{{item.status}}</ui5-badge>
```

In the end of this step, you should be able to see the following:

![Step 5 finished](./images/Step5.png?raw=true "Step 5 Result")

### [Step #6 - Detail Header](./Step6_Detail_Header.md)
