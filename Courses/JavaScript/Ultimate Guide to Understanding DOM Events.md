> Notes from [# Your Ultimate Guide to Understanding DOM Events](https://egghead.io/courses/the-ultimate-guide-for-understanding-dom-events-6c0c0d23) course on Egghead.

An event is a signal that something has occurred in the browser, the general one are the user events (clicks, hover and so on) and the system events (`DOMContentLoaded`).

The events get **dispatched** to `EventTarget`, an interface implemented by many object in the browser. When an object implement the `EventTarget` the element is able to listen for events.

The Event Handler is a name for a method added to an event listener.

The `EventTarget` can partecipate in a tree and all listen for events.

Generally the event have a corresponding target and flow in three separate phases:
* **capture phase** the event travels down through the event targets until reaching the target
* **target phase** the event hits the target and travels through the target
* **bubble phase** once completed the event goes back up 

We can add event listener for the capture or the bubble phase on each targets.

We can disable specific phase if needed.

## Listen events with HTML attribute event handlers
The fastest way to get going is using an HTML attribute, many elements have those. For example the button:
```html
<button
	onclick="console.log('you clicked')" //Bubble phase
	onload="console.log('I loaded')" //Bubble phase
>
	Button text
</button>
```
As you can see from comments, those attributes will be fired (dispatched) during the *Bubble phase*. This kind of attributes are known as *DOM0 event handler*.

They can have any JavaScript code in it and you can access on an `event` object and generally you call a function declared somewhere else and you'll get an `ReferenceError` if the function is not found since the function must live in the `window`.

Remember to **call** the function with `()` when you use the attributes 😉

## Listen events with Object Property
HTML attributes has two rapresentation:
* **content attributes** - the attribute itself
* **IDL attributes** *(Interface Description Language)* - the property corresponding to the JavaScript DOM object

Also the event handlers have a corresponding JavaScript object, for example the `onclick` attribute has the `.onclick` property handler and it listen for the bubble phase and the only have the `event` argument.

```js
button.onclick = function onClick(event){
	console.log(this, event)
}
```
The `this` context is set to the object that the event handler is bind to.

```js
button.onclick = () => {
	console.log('binded to button')
}
```
[[arrow function|Arrow functions]] locks the `this` context to its parent scope.

... to be completed
## Understand relationship between HTML attribute and Object Property
**HTML attribute**
```html
<button onclick="console.log('HTML attribute')">
	Click Me
</button>										
```
**Object Property**
```js
const button = document.querySelector('button');
button.onclick = function onClick(){
	console.log('Object Property')
}
```
You can only have one, and it depends on the last one that's declared.

We can unbound the handler with a little bit of JavaScript like with `button.onclick = null` and we can even call the `onclick` via JavaScript with `button.onclick()`.

But using events like this is really rare, almost never used because we have better way to handle this.
## Add event listener with `addEventListener`
With the `addEventListener` API we can add multiple event listener for the same element and same event. This kind of event listener are also known as *DOM2+ event listener*.
```js
// Same button const
function onClick(event){
	console.log('clicked')
}

button.addEventListener('click', onClick);
```
There are three arguments for `addEventListener`:
* *string* - the type of event we bind to
* *function* or *object* - what we need to run
* *boolean* or *config object* - if *boolean* we set the capture/bubble phase (`true` for capture and `false` for bubble) otherwise we can set options to customize event behavior `{ capture, once: false, passive: false }`

We generally pass as second argument a function, the object is just more code for same result.

The `this` in the function binds to the element that fires the event.
## Remove an Event Listener with `removeEventListener`
## Choose an Event Listener Mechanism
## The Execution Order of Event Listeners
## The Execution Order of Event Listeners in the Target Phase
## The Event Object
## Log Events to the Console
## Cancel Events
## Cancel Bespoke Events
## Stop Events
## The Event Delegation Pattern
## Create and Dispatch Synthetic Events
## Deprecated Event Creation Mechanisms
## Events are Dispatched Synchronously
## Add and Remove Event Listeners while an Event is Dispatching
## DOM Events and the Event Loop
## DOM Events and Microtasks
## Improve Scroll Performance with Passive Event Listeners
## Default Passive Values on the Body Element
## Synchronous and Asynchronous Events (Ordered and Unordered Events)
## Window Reflecting Body Element Event Handlers
## Debug and Inspect Event Listeners with Chrome Developer Tools
## Debug Event Listener Performance with Chrome Developer Tools