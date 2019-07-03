<template>
  <header class="detail-page-header">
    <div class="detail-page-header-bar">
      <ui5-title>Inventory</ui5-title>

      <ui5-button
        design="Transparent"
        icon="sap-icon://action"
        class="action-button"
      ></ui5-button>
    </div>

    <ui5-tabcontainer
      class="detail-page-header-menu"
      fixed
      collapsed
      @itemSelect="
        $emit('tabPress', $event.detail.item.getAttribute('data-filter-type'))
      "
    >
      <ui5-tab
        data-filter-type="all"
        text="All Items"
        :additional-text="products.length"
      ></ui5-tab>
      <ui5-tab
        data-filter-type="noPerishable"
        text="Non-Perishable"
        :additional-text="nonPerishableCount"
      ></ui5-tab>
      <ui5-tab
        data-filter-type="perishable"
        text="Perishable"
        :additional-text="perishableCount"
      ></ui5-tab>
      <ui5-tab
        data-filter-type="alerts"
        text="Alerts"
        :additional-text="alertCount"
      ></ui5-tab>
    </ui5-tabcontainer>
  </header>
</template>

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
