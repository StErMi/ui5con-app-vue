# Home component

In this step we will make use of ```ui5-card``` as main building block for our home view. We will create the "Featured" section. As you can see below, it consists of two "cards" - "Inventory" and "Security". Each of them has a header and content section with a list of important information.

![Home](./step2.png?raw=true "Home")

1. Create `Home.vue` file under `src/views/`.

2. Copy the `data.json` file in `src/data/`
from [Sources of Smart Store](https://github.com/stermi/ui5con-app-vue/blob/master/src/data/). The file has some mockup data, that we will need to fill into the cards.

1. Import the `ui5-card` (and other components) in `src/App.vue` to have all UI5 WebComponents imported at one place.

	```js 
	// App.vue
    import logo from "./assets/logo.png";
    import profile from "./assets/profile.png";

    import "@ui5/webcomponents/dist/ShellBar";
    import "@ui5/webcomponents/dist/Card";
    import "@ui5/webcomponents/dist/Title";
    import "@ui5/webcomponents/dist/Label";
    import "@ui5/webcomponents/dist/List";
    import "@ui5/webcomponents/dist/CustomListItem";
    import "@ui5/webcomponents/dist/StandardListItem";
	```

4. Let's start with the "Featured" section.
Create the `Home` component in `src/views/Home.vue`. Note that we import the `data.json` and set its content to the component state, so we can later use it.

	```js
	// Home.js
	<template>
        <div class="app-content">
            <ui5-title level="H3">Featured</ui5-title>
            <section class="section"></section>
        </div>
    </template>

    <script>
    import data from "../data/data.json";

    export default {
        name: "home",
        data: function() {
            return {
                data: data
            };
        }
    };
    </script>
	```

5. Now, let's add the `ui5-card`. We will also use `ui5-list` (List) and `ui5-li` (StandardListItem) for the `ui5-card` content. 
You can get familiar with the API of those components - [Card API](https://sap.github.io/ui5-webcomponents/playground/components/Card/) and [List API](https://sap.github.io/ui5-webcomponents/playground/components/List/). What is going below?
We are just using the API of the UI5 WebComponents ("heading", "subtitle" and "status") and the `v-for` syntax to map the data and the cards will render nicely.

	```html
    <template>
        <div class="app-content">
            <ui5-title level="H3">Featured</ui5-title>
            <section class="section">
            <ui5-card
                v-for="dataObj of data.featured"
                v-bind:key="dataObj.key"
                :heading="dataObj.heading"
                :subtitle="dataObj.subtitle"
                :status="dataObj.status"
                class="ui5card"
            >
                <ui5-list separators="Inner">
                    <ui5-li
                        v-for="item of dataObj.items"
                        v-bind:key="item.key"
                        :icon="item.icon"
                        :description="item.description"
                        :info="item.info"
                        :info-state="item.infoState"
                        class="ui5list-item"
                    >{{item.title}}</ui5-li>
                </ui5-list>
            </ui5-card>
            </section>
        </div>
    </template>
	```

1. Import the `Home` component into the `src/App.vue`. You should be able to see the cards with data inside. OK, the cards are currently expanded to full width and the layout does not look like the picture in the begining - we will handle it in the following step.
	```js 
	// App.vue
	<template>
        <div id="app" class="App">
            <ui5-shellbar
                primary-title="Smart Store Manager"
                show-notifications
                show-product-switch
                show-co-pilot
                :profile="profile"
                :logo="logo"
            ></ui5-shellbar>

            <Home />
        </div>
    </template>

    <script>
        import logo from "./assets/logo.png";
        import profile from "./assets/profile.png";

        import "@ui5/webcomponents/dist/ShellBar";
        import "@ui5/webcomponents/dist/Card";
        import "@ui5/webcomponents/dist/Title";
        import "@ui5/webcomponents/dist/Label";
        import "@ui5/webcomponents/dist/List";
        import "@ui5/webcomponents/dist/CustomListItem";
        import "@ui5/webcomponents/dist/StandardListItem";

        import Home from "@/views/Home.vue"

        export default {
            name: "app",
            components: {
                Home
            },
            data: function() {
                return {
                    logo: logo,
                    profile: profile
                };
            }
        };
    </script>
	```

7. The layouting and ordering of the cards is responsibility of the app developer. Replace the content of `src/App.vue <style></style>` with the content of [App.vue <style></style>](https://github.com/stermi/ui5con-app-vue/blob/master/src/App.vue). Nothing magical, we make use of `display:flex` for the layouting and setting some `min-width` to the `ui5-card`.

8. You can copy the rest of the sections in the `Home` component from [Sources of Smart Store (Home.vue)](https://github.com/stermi/ui5con-app-vue/blob/master/src/Home.vue), but don`t forget to copy all the UI5 components imports from the [Sources of Smart Store (App.vue)](https://github.com/stermi/ui5con-app-vue/blob/master/src/App.vue) as some of them are used in these cards.

### [Step #3 - The Routing](./Step3_The_Routing.md)
