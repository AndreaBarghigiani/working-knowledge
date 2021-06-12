> Random notes taken reading the [Handbook](https://stimulus.hotwire.dev/handbook/hello-stimulus).

This library is great at connecting the DOM element to a JavaScript object, also known as *controllers*.

The connection between the element and the JavaScript file is made automatically based on the name of the controller itself, [different build system have different defaults](https://stimulus.hotwire.dev/handbook/installing) but in the Handbook examples the controller will live in `src/controllers/[name]_controller.js`.

In the example above the `name` is a variable and will change based on the value we give to the `data-controller` in our HTML. 

Basically if we create the following HTML element:
```html
<div data-controller="hello">
	<!-- Other code -->
</div>
```
Stimulus will check the specific code for this element in `hello_controller.js`. To be sure about that we can do just a `console.log` fired in the `connect()` method.
```js
// src/controllers/hello_controller.js
import { Controller } from "stimulus"

export default class extends Controller {
  connect() {
    console.log("Hello, Stimulus!", this.element)
  }
}
```
## Action respond to DOM Events
One of the best feature of JS is it's ability to respond to the events we fire in the browser, after we learn how we can (automatically) connect our HTML to JS it is time to learn how to fire some fns or methods.

First and foremost let's change the name of `connect()`, method that it's called at each load, into something that we can call from the HTML:
```js
// src/controllers/hello_controller.js
import { Controller } from "stimulus"

export default class extends Controller {
  greet() {
    console.log("Hello, Stimulus!", this.element)
  }
}
```
Now we have the `greet()` method that we want to fire when the user clicks on the button in our HTML, but how can we do that?

It is just a matter of a new `data` attribute and a bit of special syntax in it:
```html
<div data-controller="hello">
  <input type="text">
  <button data-action="click->hello#greet">Greet</button>
</div>
```
You are in front of your first **action descriptors** `click->hello#greet` that we can understand like so:
* `click` is the event name
* `hello` is the controller name
* `greet` is the name of the method to invoke

## Map elements to controller properties with `target`
If you have a React background you know that if you want to connect an `input` controller to the state you can also use a `useRef`, there are other ways like make the input controlled but for the scope of this let's just considere this case.

`useRef` connects an HTML element to the JS handled by react, in Stimulus we use targets. 

To create a target we use the `data-hello-target` attribute, see how the controller name `hello` is right in the middle.
```html
<div data-controller="hello">
  <input data-hello-target="name" type="text">
  <button data-action="click->hello#greet">Greet</button>
</div>
```
Now that we have this new attribute we need to add it the the `targets` array of our controller, doing so Stimulus will create `this.nameTarget` and that will return the first matching element. We can use this property to read the element's `value`.
```js
import { Controller } from "stimulus"

export default class extends Controller {
  static targets = [ "name" ]

  greet() {
    const element = this.nameTarget
    const name = element.value
    console.log(`Hello, ${name}!`)
  }
}
```
For each item we put into the `targets` array Stimulus will generate the following:
* `this.nameTarget` - the first `name` target in the controller, if there is no `name` target an error will be thrown
* `this.nameTargets` - an array of all the `name` targets in the controller
* `this.hasNameTarget` - `true` if there is a `name` target and `false` if not.
## Refactor to simplify our controller
Since a controller is nothing more than an instance of a JavaScript class we can start to refactor a bit our controller creating a `name` getter:
```js
import { Controller } from "stimulus"

export default class extends Controller {
  static targets = [ "name" ]

  greet() {
    console.log(`Hello, ${this.name}!`)
  }

  get name() {
    return this.nameTarget.value
  }
}
```
# Create a Copy Button
The scope of this step is to create a button that will be able to copy the value of an `input`, this is our HTML starting point:
```html
<div data-controller="clipboard">
  PIN: <input type="text" value="1234" readonly>
  <button>Copy to Clipboard</button>
</div>
```
As we can see from our HTML we define a `clipboard` controller, let's create the corresponding file with a `copy()` method:
```js
// src/controllers/clipboard_controller.js
import { Controller } from "stimulus"

export default class extends Controller {
  copy() {
  }
}
```
## Define the target
As done previously it is time to connect our elements creating some `target`:
```html
<div data-controller="clipboard">
  PIN: <input data-clipboard-target="source" type="text" value="1234" readonly>
  <button>Copy to Clipboard</button>
</div>
```
Now that we have added the required `data` attribute it is time to create the target in our controller:
```js
export default class extends Controller {
  static targets = [ "source" ]
  
  // Other code...
```
## Connect the action
Time to connect the button to our action, as done before we just need the `data-action` and pass to it the method that we want to fire:
```html
<button data-action="clipboard#copy">Copy to Clipboard</button>
```
As you can see the value for `data-action` this time miss the event type, this is because common elements have a default event set for those. For the `button` the default event is `click` so instead of writing `click->clipboard#copy` we write just `clipboard#copy`.
> To see a full list of default event and the corresponding element check [the documentation](https://stimulus.hotwire.dev/handbook/building-something-real#common-events-have-a-shorthand-action-notation).

Now we can select the input field and call the clipboard API:
```js
copy() {
	this.sourceTarget.select()
	document.execCommand("copy")
}
```
## Controller are reusable by default
A powerful approach to Stimulus is the fact that a controller is reusable by default, does not matter how many elements in our page uses the same controller because each time we fire an action within one of those all it's functionality will be scoped on that precise element.

Even with this example, if we create a new container with a different value for the PIN input when the user clicks to the button will be copied the value contained in the same `data-controller` attribute.