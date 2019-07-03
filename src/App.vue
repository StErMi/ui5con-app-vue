<template>
  <div id="app" class="App">
    <AppBar />

    <router-view />
  </div>
</template>

<script>
import "@ui5/webcomponents/dist/Card";
import "@ui5/webcomponents/dist/DatePicker";
import "@ui5/webcomponents/dist/Icon";
import "@ui5/webcomponents/dist/Input";
import "@ui5/webcomponents/dist/Label";
import "@ui5/webcomponents/dist/List";
import "@ui5/webcomponents/dist/CustomListItem";
import "@ui5/webcomponents/dist/StandardListItem";
import "@ui5/webcomponents/dist/ShellBar";
import "@ui5/webcomponents/dist/Switch";
import "@ui5/webcomponents/dist/Tab";
import "@ui5/webcomponents/dist/TabContainer";
import "@ui5/webcomponents/dist/Timeline";
import "@ui5/webcomponents/dist/Option";

import AppBar from "@/views/AppBar.vue";

export default {
  name: "app",
  components: {
    AppBar
  },
  data: function() {
    return {};
  }
};
</script>

<style>
html,
body,
.root,
.App {
  padding: 0;
  margin: 0;
  background: var(--sapShell_Background, #fafafa);
}

.app-content {
  padding: 2rem;
}

.section {
  padding: 1rem 0;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.centered {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
}

.ui5card {
  align-self: flex-start;
  min-width: 18rem;
  max-width: 20rem;
  margin: 0.5rem 1rem 1rem 0;
}

.ui5card-large {
  min-width: 30rem;
  max-width: 60%;
  flex: 1 1;
}

.card-content {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}

.card-content-child {
  flex: 1 1;
}

.ui5icon-size,
.ui5label-size {
  font-size: 1.75rem;
}

.ui5list-item {
  font-size: 1.75rem;
  --sapUiContentNonInteractiveIconColor: #1a9898;
}

/* Action alerts */
.ui5card-alert {
  min-width: 12rem;
  width: 12rem;
}

.ui5card-alert-content {
  display: flex;
  align-items: flex-end;
  padding: 1rem;
  height: 6rem;
}

.ui5card-alert-icon {
  margin: 0 0.5rem;
}

/* Profile Area */
.profile-header {
  flex-direction: column;
  padding: 1rem;
}

.profile-content {
  min-width: 17rem;
  padding: 0 0.25rem;
}

.profile-img {
  width: 6rem;
  height: 6rem;
  border-radius: 50%;
}

.profile-hcb-switch {
  width: 100%;
}

/* Semantic colors */
.error {
  color: var(--sapNegativeColor, #bb0000);
}
</style>
