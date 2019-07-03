<template>
  <div class="detail-page">
    <Header
      :products="products"
      :nonPerishableCount="filterNoPerishableProducts(products).length"
      :perishableCount="filterPerishableProducts(products).length"
      :alertCount="filterAlertProducts(products).length"
      @tabPress="applyFilter"
    />

    <main class="detail-page-content">
      <FilterBar
        @filter="filter"
        @sortAsc="sortAsc"
        @sortDesc="sortDesc"
        @createProduct="createProduct"
      />

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
            Order date
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
              <b>{{ item.name }}</b>
            </ui5-label>
          </ui5-table-cell>
          <ui5-table-cell>
            <span class="table-cell-content">{{ item.price }}</span>
          </ui5-table-cell>
          <ui5-table-cell>
            <span class="table-cell-content">{{ item.location }}</span>
          </ui5-table-cell>
          <ui5-table-cell>
            <span class="table-cell-content">{{ item.orderDate }}</span>
          </ui5-table-cell>
          <ui5-table-cell>
            <span class="table-cell-content">
              <img alt class="image-cell" :src="getProductImage(item.img)" />
            </span>
          </ui5-table-cell>
          <ui5-table-cell>
            <span class="table-cell-content">
              <ui5-badge :color-scheme="getBadgeType(item.status)">{{
                item.status
              }}</ui5-badge>
            </span>
          </ui5-table-cell>
        </ui5-table-row>
      </ui5-table>
    </main>
  </div>
</template>

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

import Header from "@/views/Header.vue";
import FilterBar from "@/views/FilterBar.vue";

export default {
  name: "detail",
  components: {
    Header,
    FilterBar
  },
  data: function() {
    return {
      products: [...products],
      filteredProducts: [...products],
      filterType: "all",
      statusCriteriaMapping: {
        "In-Stock": 0,
        "Re-Stock": 1,
        Deterioating: 2
      }
    };
  },
  methods: {
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
    },

    getProductImage(img) {
      return img.startsWith("http") ? img : require(`@/assets${img}`);
    },

    filterPerishableProducts(items) {
      return items.filter(product => product.perishable);
    },

    filterNoPerishableProducts(items) {
      return items.filter(product => !product.perishable);
    },

    filterAlertProducts(items) {
      return items.filter(
        product =>
          product.status === "Deterioating" || product.status === "Re-Stock"
      );
    },

    applyFilter(filterType) {
      const products = this.filterItems(filterType, this.products);
      this.filteredProducts = products;
      this.filterType = filterType;
    },

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
    },

    sortAsc() {
      const sortedItems = this.filteredProducts.sort((a, b) => {
        if (
          this.statusCriteriaMapping[a.status] >
          this.statusCriteriaMapping[b.status]
        ) {
          return 1;
        } else if (
          this.statusCriteriaMapping[a.status] <
          this.statusCriteriaMapping[b.status]
        ) {
          return -1;
        }

        return 0;
      });

      this.filteredProducts = sortedItems;
    },

    sortDesc() {
      const sortedItems = this.filteredProducts.sort((a, b) => {
        if (
          this.statusCriteriaMapping[a.status] >
          this.statusCriteriaMapping[b.status]
        ) {
          return -1;
        } else if (
          this.statusCriteriaMapping[a.status] <
          this.statusCriteriaMapping[b.status]
        ) {
          return 1;
        }

        return 0;
      });

      this.filteredProducts = sortedItems;
    },

    filterVisibleItemsByText(text) {
      const filteredByType = this.filterItems(this.filterType, this.products); // filter items based on current filter type
      const items = filteredByType.filter(item =>
        item.name.toLowerCase().startsWith(text)
      ); // filter items based on starting text

      this.filteredProducts = items;
    },

    filter(value) {
      this.filterVisibleItemsByText(value);
    },

    createProduct(entry) {
      const newItems = [
        ...this.products,
        { key: this.products.length + 1, ...entry }
      ];

      this.products = newItems;
      this.filteredProducts = this.filterItems(this.filterType, newItems);
    }
  }
};
</script>

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
