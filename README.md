# Smart Store app Vue.js

The [Smart Store app](https://ilhan007.github.io/ui5con-app) is a [Vue.js](https://vuejs.org/) sample application, demonstrating the usage of the [UI5 Web Components](https://github.com/SAP/ui5-webcomponents). You can find a step by step tutorial below on how to build the app by yourself. You don't have to clone the repo, the app will be built from scratch.

Note: no previous experience with [UI5 Web Components](https://github.com/SAP/ui5-webcomponents) is required.
To take the most of the tutorial, a basic knowledge with [Vue.js](https://vuejs.org/) is desirable.

## Prerequisites
- [Node.js](https://nodejs.org/) - **version 8.10 or later** (check the version with `node -v`)
- npm - **version 6 or later** (check the version with `npm -v`)
- [Vue CLI](https://cli.vuejs.org/) - 🛠️ Standard Tooling for Vue.js Development

## Short story

The Smart Store app enables a store manager to control his/her multiple stores. It provides the most important information and status of the stores and urgent tasks that should be addressed by the store manager.

## Getting started

1. Bootstrap the app with [Create Vue.js App](https://vuejs.org/v2/guide/) executing the following commands in your terminal.

	```sh
	vue create smart-store-app
	cd smart-store-app
	```

2. Install the UI5 Web Components.

	```sh
	npm install @ui5/webcomponents
	```

3. Consume the UI5 Web Components.

	All the components can be imported from `"@ui5/webcomponents/dist/<component_name>";`
	Import one of the available components in the `src/App.vue`.
	```js
	import "@ui5/webcomponents/dist/Button"; // loads ui5-button
	```
 
	Then, you can add the `ui5-button` in `src/App.vue` and that`s it! 

	```js
	export default {
		name: "app",
		template: `<ui5-button>Hello world!</ui5-button>`,
		data: function() {}
	};
	```
4. Launch the app and you should see the `ui5-button` rendered on the screen.
	```sh
	npm run serve
	```
## Build the Smart Store app

### [Step #1 - The App Bar](./docs/Step1_The_App_Bar.md)
### [Step #2 - The Home Component](./docs/Step2_The_Home_Component.md)
### [Step #3 - The Routing](./docs/Step3_The_Routing.md)
### [Step #4 - The Profile Area](./docs/Step4_The_Profile_Area.md)
### [Step #5 - Detail Page](./docs/Step5_Details.md)
### [Step #6 - Detail Page Header](./docs/Step6_Detail_Header.md)
### [Step #7 - Detail Page Filter Bar](./docs/Step7_Detail_FilterBar.md)
### [Step #8 - Detail Page Create Item Dialog](./docs/Step8_Detail_add_new_item.md)

## Resources
[Smart Store app React (original)](https://ilhan007.github.io/ui5con-app)

[Sources of Smart Store App React (original)](https://github.com/ilhan007/ui5con-app)

[List of all available UI5 Web Components](https://sap.github.io/ui5-webcomponents/playground)
