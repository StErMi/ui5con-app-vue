<template>
  <div class="app-content">
    <ui5-title level="H3">Featured</ui5-title>
    <section class="section">
      <ui5-card
        v-for="dataObj of data.featured"
        :key="dataObj.key"
        :heading="dataObj.heading"
        :subtitle="dataObj.subtitle"
        :status="dataObj.status"
        header-interactive
        @headerClick="navToDetail"
        class="ui5card"
      >
        <ui5-list separators="Inner">
          <ui5-li
            v-for="item of dataObj.items"
            :key="item.key"
            :icon="item.icon"
            :description="item.description"
            :info="item.info"
            :info-state="item.infoState"
            class="ui5list-item"
            >{{ item.title }}</ui5-li
          >
        </ui5-list>
      </ui5-card>
    </section>

    <ui5-title level="H3">Today at a glance</ui5-title>
    <section class="section">
      <ui5-card
        heading="Upcoming Activities"
        subtitle="28 Jun 2019"
        class="ui5card"
      >
        <ui5-timeline>
          <ui5-timeline-item
            v-for="item of data.activities"
            :key="item.key"
            :icon="item.icon"
            :title-text="item.title"
            :subtitle-text="item.subtitle"
            class="ui5list-item"
          >
            <div>{{ item.location }}</div>
          </ui5-timeline-item>
        </ui5-timeline>
      </ui5-card>

      <ui5-card
        heading="Energy Efficiency"
        subtitle="Smart Store A"
        class="ui5card"
      >
        <ui5-list separators="Inner">
          <ui5-li
            v-for="item of data.energystats"
            :key="item.key"
            :icon="item.icon"
            :description="item.description"
            :info="item.info"
            class="ui5list-item"
            >{{ item.title }}</ui5-li
          >
        </ui5-list>
      </ui5-card>

      <ui5-card
        avatar="sap-icon://retail-store"
        heading="Smart Stores"
        subtitle="Bulgaria"
        status="6 of 6"
        class="ui5card ui5card-large"
      >
        <div class="card-content">
          <ui5-list separators="Inner" class="card-content-child">
            <ui5-li
              v-for="store of data.storesa"
              :key="store.key"
              :image="managerImg"
              :description="store.description"
              >{{ store.title }}</ui5-li
            >
          </ui5-list>
          <ui5-list separators="Inner" class="card-content-child">
            <ui5-li
              v-for="store of data.storesb"
              :key="store.key"
              :image="managerImg"
              :description="store.description"
              >{{ store.title }}</ui5-li
            >
          </ui5-list>
        </div>
      </ui5-card>
    </section>

    <ui5-title level="H3">Action Required</ui5-title>
    <section class="section">
      <ui5-card
        v-for="action of data.actions"
        :key="action.key"
        heading="Smart Store 1"
        subtitle="today"
        status="3 of 6"
        class="ui5card ui5card-large"
      >
        <ui5-table>
          <ui5-table-column
            v-for="column of action.columns"
            :key="column.key"
            slot="columns"
          >
            <div>
              <ui5-label>{{ column.name }}</ui5-label>
            </div>
          </ui5-table-column>
          <ui5-table-row v-for="dataObj of action.rows" :key="dataObj.key">
            <ui5-table-cell v-for="cell of dataObj.cells" :key="cell.key">
              <ui5-label :class="cell.error">{{ cell.text }}</ui5-label>
            </ui5-table-cell>
          </ui5-table-row>
        </ui5-table>
      </ui5-card>
      <ui5-card
        v-for="alert of data.alerts"
        :key="alert.key"
        :heading="alert.heading"
        :subtitle="alert.subtitle"
        class="ui5card ui5card-alert"
      >
        <div class="ui5card-alert-content">
          <ui5-icon
            :src="alert.icon"
            class="ui5icon-size ui5card-alert-icon"
          ></ui5-icon>
          <ui5-label class="ui5label-size error">{{ alert.text }}</ui5-label>
        </div>
      </ui5-card>
    </section>
  </div>
</template>

<script>
import data from "../data/data.json";
import managerImg from "../assets/profile.png";

import "@ui5/webcomponents/dist/Card";

export default {
  name: "home",
  data: function() {
    return {
      data: data,
      managerImg: managerImg
    };
  },
  methods: {
    navToDetail() {
      this.$router.push({ name: "detail" });
    }
  }
};
</script>
