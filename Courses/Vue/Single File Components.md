A comoponent build with this approach will have the following structure:
* `template` - the structure
* `script` - the brain
* `style` - the dress

Instead to create different files Vue allows us to write all we need into a single file, this allows us to easily swap templating language simply adding the `lang` attribute to this elements:
```vue
<template lang="pug">
	// As in React I need one root component
	// With Vue 2 you have to add a <div> 
</template>

<script lang="ts">

</script>

<style lang="scss">

</style>
```
Even if this seems simple you still have to setup the proper loaders and configure Webpack to understand new syntax, this is easier thanks to the Vue CLI because allows us to simply install them via plugin.
![[vue-sass-loader.png]]
The screenshot is taken from the Vue UI, that you can call with `vue ui`, and for all the common settings you can have a look at [the docs](https://vue-loader.vuejs.org/guide/pre-processors.html).

The `export default` in our `<script>` allows us to make the component we're building available to all the application, this means that we can import it in any other components. 

In order to use it in an external component though we first need to import it in the component and then make it available inside the `export default` of that specific component.

This sound cumbersome so let's make and example.

We created the `EventCard` component and we want to use it inside the `EventList` one, inside the `EventList.vue` we will write the following:
```vue
// Other part of the component
<script>
import EventCard from "@/components/EventCard"
export default {
  components: {
    EventCard,
  },
}
</script>
```
Once we've done that we can use it inside our `template` like so:
```vue
<template>
  <div>
	<EventCard/>
  </div>
</template>
```