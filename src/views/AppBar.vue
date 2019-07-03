<template>
  <div class="app-bar">
    <ui5-shellbar
      primary-title="Smart Store Manager"
      show-notifications
      show-product-switch
      show-co-pilot
      :profile="profile"
      :logo="logo"
      @profileClick="onProfileClicked"
    ></ui5-shellbar>

    <ui5-popover
      id="profile-popover"
      ref="profile-popover"
      hide-header
      placement-type="Bottom"
      horizontal-align="Right"
    >
      <div class="profile-header centered">
        <img :src="profile" alt class="profile-img" />
        <ui5-title level="3">Darius Cummings</ui5-title>
        <ui5-label>Store Manager</ui5-label>
      </div>

      <div class="profile-content">
        <ui5-list separators="None">
          <ui5-li-custom type="Inactive">
            <div class="profile-hcb-switch centered">
              <ui5-li icon="sap-icon://palette" type="Inactive"
                >High Contrast Black</ui5-li
              >
              <ui5-switch @change="onThemeSwitchPressed"></ui5-switch>
            </div>
          </ui5-li-custom>
          <ui5-li icon="sap-icon://settings">Settings</ui5-li>
          <ui5-li icon="sap-icon://sys-help">Help</ui5-li>
          <ui5-li icon="sap-icon://log">Sign out</ui5-li>
        </ui5-list>
      </div>
    </ui5-popover>
  </div>
</template>

<script>
import logo from "../assets/logo.png";
import profile from "../assets/profile.png";

import { setTheme } from "@ui5/webcomponents-base/Theming";
import "@ui5/webcomponents/dist/ThemePropertiesProvider";

export default {
  name: "app-bar",
  data: function() {
    return {
      logo: logo,
      profile: profile
    };
  },
  methods: {
    onProfileClicked(event) {
      this.$refs["profile-popover"].openBy(event.detail.targetRef);
    },

    onThemeSwitchPressed(event) {
      setTheme(event.target.checked ? "sap_belize_hcb" : "sap_fiori_3");
    }
  }
};
</script>
