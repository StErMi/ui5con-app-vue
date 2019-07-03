# Detail Page Adding a new item

Last action we will implement is adding a new item to our list of items.

![Add Item Dialog](./images/add-dialog.png?raw=true "Add Item Dialog")


We will enhance our `FilterBar` component to show a dialog inside.

You can place the following markup anywhere in your Filter Bar's renderer (except top level).
Note: Don't forget to add the following `import "@ui5/webcomponents/dist/Option";` in `src/App.vue` as it is used as an item of the `ui5-select`.

```html
<ui5-dialog header-text="Add a new product" ref="dialog">
    <div class="dialog-content">
        <div class="dialog-section">
            <ui5-label>Product name:</ui5-label>
            <ui5-input ref="nameInput"></ui5-input>
        </div>

        <div class="dialog-section">
            <ui5-label>Product price:</ui5-label>
            <ui5-input ref="priceInput"></ui5-input>
        </div>

        <div class="dialog-section">
            <ui5-label>Product location:</ui5-label>
            <ui5-textarea ref="locationInput" show-exceeded-text max-length="100"></ui5-textarea>
        </div>

        <div class="dialog-section">
            <ui5-label>Order date:</ui5-label>
            <ui5-datepicker ref="dateInput"></ui5-datepicker>
        </div>

        <div class="dialog-section">
            <ui5-label>Image URL:</ui5-label>
            <ui5-input ref="imageInput" type="URL" placeholder="https://..."></ui5-input>
        </div>

        <div class="dialog-section">
            <ui5-label>Status:</ui5-label>

            <ui5-select ref="statusInput">
                <ui5-option>In-Stock</ui5-option>
                <ui5-option>Re-Stock</ui5-option>
                <ui5-option>Deterioating</ui5-option>
            </ui5-select>
        </div>
        <div class="dialog-section horizontal-flex">
            <ui5-radiobutton selected name="perishable" text="Perishable" ref="rbPerishable"></ui5-radiobutton>
            <ui5-radiobutton name="perishable" text="Non-Perishable"></ui5-radiobutton>
        </div>
    </div>

    <div slot="footer" class="dialog-footer">
        <ui5-button design="Emphasized" @click="submitNewProduct">OK</ui5-button>
        <ui5-button @click="closeDialog">Cancel</ui5-button>
    </div>
</ui5-dialog>
```

Update the `CSS` template of the `FilterBar`

```css
/* Detail Page Dialog */
.dialog-footer {
	padding: 0.25rem;
	display: flex;
	justify-content: flex-end;
}

.dialog-footer > ui5-button:first-child {
	margin-right: 0.5rem;
}

.dialog-section {
	display: flex;
	margin-top: 1rem;
	flex-direction: column;
	width: 100%;
}

.dialog-section:not(.horizontal-flex) > *:not(:first-child) {
	margin-top: 0.5rem;
}

.dialog-content {
	display: flex;
	flex-direction: column;
	width: 320px;
	align-items: center;
	padding: 1rem 1rem 1.5rem 1rem;
}

.horizontal-flex {
	flex-direction: row;
}
```

In order to open this dialog, you need to just call the method `.open` on the DOM element `ui5-dialog`. We will do this in the `click` handler of the Create `ui5-button`.

```html
<ui5-button design="Transparent" title="Create Product" @click="openDialog">Create</ui5-button>
```

```js
openDialog() {
	this.$refs.dialog.open();
}
```

When the OK `ui5-button` is clicked, we should create a new item, when the `Cancel` button is clicked, we should close the `ui5-dialog`. Closing the `ui5-dialog` is the same as opening it - you just call a single method called `close`. You can read more about the Dialog's API [here](https://sap.github.io/ui5-webcomponents/playground/components/Dialog/).

```js
closeDialog() {
	this.$refs.dialog.close();
}
```

As we have defined `ref`s for all of the components inside the `ui5-dialog` we can read all of their current values. Therefore, we will create a method called `submitNewProduct` which gathers the information from the fields and calls the `createProduct` passed as a property to the `FilterBar` from the `Detail` component. 

```js
submitNewProduct() {
    let newEntry = {
        name: this.$refs.nameInput.value,
        price: this.$refs.priceInput.value,
        location: this.$refs.locationInput.value,
        img: this.$refs.imageInput.value,
        status: [].filter.call(this.$refs.statusInput.children, el => el.selected)[0].textContent,
        orderDate: this.$refs.dateInput.value,
        perishable: !!this.$refs.rbPerishable.selected
    };

    this.$emit('createProduct', newEntry);
    this.closeDialog();
}
```

`createProduct` will modify the state based on the collected information from the input fields.

Lets first pass the property to the `FilterBar` component

```html
<FilterBar
	@createProduct="createProduct"
	...
/>
```

In the `createProduct` we will receive an object containing all the information for the new item to be added to our product list.

```js
createProduct(entry) {
    const newItems = [...this.products, { key: (this.products.length + 1), ...entry }];

    this.products = newItems;
    this.filteredProducts = this.filterItems(this.filterType, newItems);
}
```

