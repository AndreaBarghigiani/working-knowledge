Vue has it's own route, different from React where you pick your favorite flavor.

When creating an app with Vue CLI, the router is installed in `src/router.js`. We need to import in this file the components that we want to use as pages that will be rendered on a specific `path`.

Once imported the `Router` from `vue-router` creating a new path is pretty easy, generally all this code is ready from the CLI but it is useful to have a look at:
```js
import Vue from "vue"
import Router from "vue-router"

// Import components as pages
import Home from "./views/Home.vue" // notice we keep extension

Vue.use(Router) // We tell Vue to use the Router

export default new Router({
	routes: [
		{
			path: "/", // The URL
			name: "home", // Name of the route
			component: Home // The component we want to render
		},
		// Other routes
	]
})
```
As a best practice the components that we use as pages are stored in `views/` folder.

`router.js` defines the behavior for the routes of our app but is in `main.js` that it gets actually loaded into our application:
```js
// Other imports
import router from "./router"

new Vue({
	router,
	// Other configs
}).$mount("#app") // Like ReactDOM.render
```
Until now we saw the configuration and the loading but in order to use the new path and make our app aware of what needs to render we need to use the `router-link` component, like the `Link` that we get from React Router.

Inside the `App.vue` we find our `nav` that uses them:
```html
<div id="nav">
	<router-link to="/">Home</router-link> |
	<router-link to="/about">About</router-link>
</div>
<router-view/>
```
Here we see also the `router-view` component that basically renders whichever component you set for that specific route. For example if you visit `/` Vue will show the `Home` component.

 ![[router-view.png]]
 
 ## Named routes
 We can use the `name` property we set in the obj of each route so we do not have to remember the exact `path`, let's change the previous `router-link` to use it:
 ```html
<router-link :to="{name: 'home'}">Home</router-link> |
<router-link :to="{name: 'about'}">About</router-link>
```
This also allow us to change the route whenever we like without thinking about updating the `router-link` in use.

### Implementing a redirect
Vue Router gives us the ability to add redirects out of the box, let's say that we change `/about` in `/about-us`. All we have to do is add the redirect to the config obj of the router like so:
```js
export default new Router({
	routes: [
		// Other routes
		{
			path: "/about-us", // Changed from /about
			name: "about",
			component: About 
		},
		{
			path: "/about", // The old path
			redirect: { name: about } // Whatever the new route is
		},
	]
})
```
### Alias, the redirect alternative
This time instead of adding a new obj we just modify the current one
```js
export default new Router({
	routes: [
		// Other routes
		{
			path: "/about-us", // Changed from /about
			name: "about",
			component: About,
			alias: "/about"
		},
	]
})
```
Using `alias` does not change the URL, if user go to `/about` the will see the `About` content but the URL **will not** change to `/about-us`.

## Automatic code splitting
Vue Router offers us the ability to create automatic code splitting based on the routes, in this case instead to `import` and pass the component to the `component` property we just need to pass a function:
```js
const routes = [
  {
    path: "/show",
    name: "Show",
    component: () =>
      import(/* webpackChunkName: "show" */ "../views/EventShow.vue"),
  },
  // Other routes
];
```
## Dynamic routes
Building dynamic routes is pretty similar to the React Router approach, the dynamic part of your route (the parameter you want to pass) is defined by `:`, so when you define your routes you can do something like this:
![[vue-dynamic-routes-definition.png]]
To access to this information you can call the value stored in the parameter with the special `$route` obj. So if you want to access to the `:username` that we defined before we can write `$route.params.username` inside the component that is in charge of rendering that specific route.

In order to link a dynamic route we need to pass an additional obj to our `router-link` component:
```html
<router-link :to="{name: 'user', { params: { username: 'Andrea' } } }">About</router-link>
```
But this can limit our options, there is a more modular way to accomplish the same result. We can send our parameters as props just by changing the definition of our route.
![[vue-route-params-props.png]]
We then need to change the `User` component definition to let it know that we accept that param:
```vue
<template>
  <div class="user">
	<h1>{{ username }}</h1>
  </div>
</template>

<script>
export default {
  props: ["username"] // Now the component knows
};
</script>
```
## Get rid of the hash mode
The router adds the `#` by default, to clean our URL we need to change the configuration of it:
![[vue-router-history-mode.png]]
This requires a [server config](https://router.vuejs.org/guide/essentials/history-mode.html#example-server-configurations) even to handle the display of the error page.