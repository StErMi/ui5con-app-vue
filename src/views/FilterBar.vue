<template>
  <div class="details-page-filter-bar">
    <ui5-title level="H3">Products</ui5-title>

    <div class="details-page-filter-bar-actions">
      <ui5-input
        class="details-page-searchfield"
        placeholder="Search"
        @input="$emit('filter', $event.target.value)"
      >
        <ui5-icon slot="icon" src="sap-icon://search"></ui5-icon>
      </ui5-input>

      <ui5-button
        design="Transparent"
        title="Create Product"
        @click="openDialog"
        >Create</ui5-button
      >
      <ui5-button
        icon="sap-icon://sort-descending"
        design="Transparent"
        title="Sort By Status"
        @click="$emit('sortDesc')"
      ></ui5-button>
      <ui5-button
        icon="sap-icon://sort-ascending"
        design="Transparent"
        title="Sort By Status"
        @click="$emit('sortAsc')"
      ></ui5-button>
      <ui5-button
        icon="sap-icon://excel-attachment"
        design="Transparent"
      ></ui5-button>
    </div>

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
          <ui5-textarea
            ref="locationInput"
            show-exceeded-text
            max-length="100"
          ></ui5-textarea>
        </div>

        <div class="dialog-section">
          <ui5-label>Order date:</ui5-label>
          <ui5-datepicker ref="dateInput"></ui5-datepicker>
        </div>

        <div class="dialog-section">
          <ui5-label>Image URL:</ui5-label>
          <ui5-input
            ref="imageInput"
            type="URL"
            placeholder="https://..."
          ></ui5-input>
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
          <ui5-radiobutton
            selected
            name="perishable"
            text="Perishable"
            ref="rbPerishable"
          ></ui5-radiobutton>
          <ui5-radiobutton
            name="perishable"
            text="Non-Perishable"
          ></ui5-radiobutton>
        </div>
      </div>

      <div slot="footer" class="dialog-footer">
        <ui5-button design="Emphasized" @click="submitNewProduct"
          >OK</ui5-button
        >
        <ui5-button @click="closeDialog">Cancel</ui5-button>
      </div>
    </ui5-dialog>
  </div>
</template>

<script>
export default {
  name: "detail-filter-bar",
  data: function() {
    return {};
  },
  methods: {
    openDialog() {
      this.$refs.dialog.open();
    },

    closeDialog() {
      this.$refs.dialog.close();
    },

    submitNewProduct() {
      let newEntry = {
        name: this.$refs.nameInput.value,
        price: this.$refs.priceInput.value,
        location: this.$refs.locationInput.value,
        img: this.$refs.imageInput.value,
        status: [].filter.call(
          this.$refs.statusInput.children,
          el => el.selected
        )[0].textContent,
        orderDate: this.$refs.dateInput.value,
        perishable: !!this.$refs.rbPerishable.selected
      };

      this.$emit("createProduct", newEntry);
      this.closeDialog();
    }
  }
};
</script>

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
</style>
