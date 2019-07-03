# Detail Page Filter Bar

One more step is left to complete our application.
Most of SAP's applications have different options for filtering.
We will now implement a simple filter bar for ordering and adding fields to our table.

Lets add a new component called FilterBar as we already did in the last step.

Once we boostrap it - add it to the `Detail Page`

```html
<Header 
	...
/>

<main class="detail-page-content">

	<FilterBar />

	<ui5-table>
	...
```

![Details Filter Bar](./images/filterbar.png?raw=true "Details Filter Bar")

Our filter bar displays:
 - `ui5-title` Products
 - `ui5-input` with a `ui5-icon` inside so we can compose a search field
 - `ui5-button` Create which opens a `ui5-dialog` for creating an item
 - `ui5-button` for ascending sorting
 - `ui5-button` for descending sorting
 - `ui5-button` with a `sap-icon://excel-attachment` icon


Lets quickly represent the mentioned above with an HTML

```html
<template>
  <div class="details-page-filter-bar">
    <ui5-title level="H3">Products</ui5-title>

    <div class="details-page-filter-bar-actions">
      <ui5-input class="details-page-searchfield" placeholder="Search">
        <ui5-icon slot="icon" src="sap-icon://search"></ui5-icon>
      </ui5-input>

      <ui5-button design="Transparent" title="Create Product">Create</ui5-button>
      <ui5-button icon="sap-icon://sort-descending" design="Transparent" title="Sort By Status"></ui5-button>
      <ui5-button icon="sap-icon://sort-ascending" design="Transparent" title="Sort By Status"></ui5-button>
      <ui5-button icon="sap-icon://excel-attachment" design="Transparent"></ui5-button>
    </div>
  </div>
</template>
```

We will again add a few styles in order to prettify our layouting.

```css
<style scoped>
.details-page-filter-bar {
	flex-wrap: wrap;
	display: flex;
	justify-content: space-between;
}

.detail-page-header-menu {
	border: 1px solid var(--sapUiListTableGroupHeaderBorderColor, #d9d9d9);
}

.details-page-filter-bar-actions {
	display: flex;
	flex: auto;
	justify-content: flex-end;
	padding-bottom: 0.25rem;
	max-width: 100%;
	flex-wrap: wrap;
}

.details-page-filter-bar-actions > *:not(:last-child) {
	margin-right: 0.25rem;
}

.details-page-searchfield {
	width: auto;
}
</style>
```

Once we get the layouting and styles done, lets move to interaction.
We will implement 4 main actions for our component:
- filtering by search criteria
- ordering descending by status
- ordering ascending by status
- creating an item (we will do this in the next step)

As we did in the last step, we should first implement these actions in the `Detail` component and propagate them as properties to the `FilterBar` component.

Lets pass the following in the `Detail`'s `template`:

```html
<FilterBar 
    @filter="filter"
    @sortAsc="sortAsc"
    @sortDesc="sortDesc"
/>
```

and import the component like this

```js

import FilterBar from "@/views/FilterBar.vue";

export default {
  name: "detail",
  components: {
    Header,
    FilterBar
  },
  data: function() {
    return {
      ...
    };
  },
  methods: {
    ...
  }
};

```

### filter

```js
filterVisibleItemsByText(text) {
	const filteredByType = this.filterItems(this.filterType, this.products); // filter items based on current filter type
	const items = filteredByType.filter(item => item.name.toLowerCase().startsWith(text)); // filter items based on starting text

	this.filteredProducts = items;
}

filter(value) {
	this.filterVisibleItemsByText(value);
}
```


### sortAsc

```js
 // add to the data state a map to map statuses to numbers (suitable for sorting)
data: function() {
    return {
        ...
        statusCriteriaMapping: {
            "In-Stock": 0,
            "Re-Stock": 1,
            Deterioating: 2
        }
        ...
    };
}

sortAsc() {
	const sortedItems = this.filteredProducts.sort((a, b) => {
		if (this.statusCriteriaMapping[a.status] > this.statusCriteriaMapping[b.status]) {
			return 1;
		} else if (this.statusCriteriaMapping[a.status] < this.statusCriteriaMapping[b.status]) {
			return -1;
		}

		return 0;
	});

	this.filteredProducts = sortedItems;
}
```

### sortDesc

```js
sortDesc() {
	const sortedItems = this.filteredProducts.sort((a, b) => {
		if (this.statusCriteriaMapping[a.status] > this.statusCriteriaMapping[b.status]) {
			return -1;
		} else if (this.statusCriteriaMapping[a.status] < this.statusCriteriaMapping[b.status]) {
			return 1;
		}

		return 0;
	});

	this.filteredProducts = sortedItems;
}
```

Once we have all the logic to be applied on the state, we should use the bound properties in the `FilterBar` component and attach the actions to the DOM elements.
For the `ui5-button`s we can use Vue's native `click` handling.

```html
<template>
  <div class="details-page-filter-bar">
    <ui5-title level="H3">Products</ui5-title>

    <div class="details-page-filter-bar-actions">
      <ui5-input class="details-page-searchfield" placeholder="Search" @input="$emit('filter', $event.target.value)">
        <ui5-icon slot="icon" src="sap-icon://search"></ui5-icon>
      </ui5-input>

      <ui5-button design="Transparent" title="Create Product">Create</ui5-button>
      <ui5-button icon="sap-icon://sort-descending" design="Transparent" title="Sort By Status" @click="$emit('sortDesc')"></ui5-button>
      <ui5-button icon="sap-icon://sort-ascending" design="Transparent" title="Sort By Status" @click="$emit('sortAsc')"></ui5-button>
      <ui5-button icon="sap-icon://excel-attachment" design="Transparent"></ui5-button>
    </div>
  </div>
</template>
```

![Filtered By Text](./images/filteredbytext.png?raw=true "Filtered By Text")

### [Step #8 - Adding an Item](./Step8_Detail_add_new_item.md)
