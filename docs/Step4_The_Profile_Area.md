# Profile Area

What is an admin UI without a profile area? We will create one for our smart store manager and let him change the application theming with one click!

![Profile Area](./step4.png?raw=true "Profile Area")

1. We will enhance our app bar for this purpose and make it a separate component. 
- Create `AppBar.vue` in `src/views`.
- Create the `AppBar` component.

	```js
	// AppBar.vue
	<template>
        <div class="app-bar">
            <ui5-shellbar
            primary-title="Smart Store Manager"
            show-notifications
            show-product-switch
            show-co-pilot
            :profile="profile"
            :logo="logo"
            ></ui5-shellbar>
        </div>
    </template>

    <script>
        import logo from "../assets/logo.png";
        import profile from "../assets/profile.png";

        export default {
            name: "app-bar",
            data: function() {
                return {
                    logo: logo,
                    profile: profile
                };
            },
            methods: {}
        };
    </script>

	```

2. Don't forget to update the `App.vue`

	```js
	// App.vue
	<template>
        <div id="app" class="App">
            <AppBar />

            <router-view/>
        </div>
    </template>

    <script>

    import "@ui5/webcomponents/dist/ShellBar";
    import "@ui5/webcomponents/dist/Card";
    import "@ui5/webcomponents/dist/Title";
    import "@ui5/webcomponents/dist/Label";
    import "@ui5/webcomponents/dist/List";
    import "@ui5/webcomponents/dist/CustomListItem";
    import "@ui5/webcomponents/dist/StandardListItem";

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
	```

3. Now, let's add the profile popover. We will use the `ui5-popover` that will open, when the `ui5-shellbar` `profileClick` event is fired, e.g. when someone clicks on the profile image.

- Add ref to the `ui5-shellbar`.
- Bind for the `profileClick` event to the `onProfileClicked` method like this `@profileClick="onProfileClicked"`.
- Add the `import "@ui5/webcomponents/dist/Popover";` and `import "@ui5/webcomponents/dist/Label";` imports among the other in `src/App.vue`.
- Open the `ui5-popover` in the listener `onProfileClicked`.

	```js
	// AppBar.js
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
            horizontal-align="Right">
            <div class="profile-header centered">
                <img :src="profile" alt class="profile-img" />
                <ui5-title level="3">Darius Cummings</ui5-title>
                <ui5-label>Store Manager</ui5-label>
            </div>

            <div class="profile-content">
                <ui5-list separators="None">
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
            }
        }
    };
    </script>
	```

	Now, you should be able to open the profile area by clicking the profile image!

1. Add the theme switch. By default the UI5 WebComponents come with Fiori 3 (known as SAP Quartz), but a high-contrast theme is also supported. To switch to another theme, you can use the framework method `setTheme`  from `@ui5/webcomponents-base/src/Theming`.
We will use the `ui5-switch` component to switch between Fiori 3 and High Contrast Black.

- Add the `import "@ui5/webcomponents/dist/Switch";` import in `src/App.vue`
- Add the `import "@ui5/webcomponents/dist/ThemePropertiesProvider"`; to enable dynamic theme switching
- Add the `import { setTheme } from "@ui5/webcomponents-base/src/Theming"`; in `src/views/AppBar.vue`
- Bind for the `ui5-switch` `change` event
- Switch the theme in the event listener `onThemeSwitchPressed`

	```js
	// AppBar.js
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
            horizontal-align="Right">
            <div class="profile-header centered">
                <img :src="profile" alt class="profile-img" />
                <ui5-title level="3">Darius Cummings</ui5-title>
                <ui5-label>Store Manager</ui5-label>
            </div>

            <div class="profile-content">
                <ui5-list separators="None">
                <ui5-li-custom type="Inactive">
                    <div class="profile-hcb-switch centered">
                    <ui5-li icon="sap-icon://palette" type="Inactive">High Contrast Black</ui5-li>
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

    import { setTheme } from "@ui5/webcomponents-base/src/Theming";
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
	```

[Step #5 - Detail Page](./Step5_Details.md)
