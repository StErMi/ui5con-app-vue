# App Routing

In this step we will create an empty Detail component and set up a routing and navigation between the Home and Detail component.


1. Create `Detail.vue` file under `src/views/`.

2. Create the `Detail` component, that will return just the words "Hello World" for now.

	```js 
	// Detail.js
	<template>
        <div class="detail-page">Hello World</div>
    </template>

    <script>
        export default {
            name: "detail",
            components: {},
            data: function() {
                return {};
            },
            methods: {}
        };
    </script>
	```
3. Install the `vue-router`.
	```js
	npm install vue-router --save
	```

4. Update the `App.vue` and replace the `<Home />` tag in the template with `<router-view/>` and remove all the Home component references.
5. Create the router configuration file in `src/router.js` with the following content:

	```js 
	// src/router.js
	import Vue from "vue";
    import Router from "vue-router";
    import Home from "./views/Home.vue";
    import Detail from "./views/Detail.vue";

    Vue.use(Router);

    export default new Router({
        mode: "history",
        base: process.env.BASE_URL,
        routes: [
            {
                path: "/",
                name: "home",
                component: Home
            },
            {
                path: "/detail",
                name: "detail",
                component: Detail
            }
        ]
    });
	```
6. Update the `main.js` configuration file to let the Vue app use the new router configuraton like this:
    
	```js 
    // src/main.js
    import Vue from 'vue'
    import App from './App.vue'
    import router from './router'

    Vue.config.productionTip = false

    new Vue({
        router,
        render: h => h(App)
    }).$mount('#app')
    ```

7. Now, let's navigate to the `Detail` component by clicking the header of our "Inventory" card.  This would require changes in the `Home` component. 

- Bind for the ui5-card `headerClick` event to the method `navToDetail` like this `@headerClick="navToDetail"`
- Add the method `navToDetail` to the list of implemented methods. It will just need to redirect the browser to the `Detail` route we have defined inside the router config `src/router.js`

	```js 
	// src/views/Home.vue
	<script>
    import data from "../data/data.json";

    export default {
        name: "home",
        data: function() {
            return {
                data: data
            };
        },
        methods: {
            navToDetail() {
                this.$router.push({ name: 'detail' })
            }
        }
    };
    </script>
	```

	Now, you can click the header of the "Inventory" card and navigate to the `Detail` component.

### [Step #4 - The Profile Area](./Step4_The_Profile_Area.md)
