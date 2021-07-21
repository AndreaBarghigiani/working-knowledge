The `controller` is the entry point in the Stimulus architecture, as in Rails, is responsible for the decision. The controller gets connected to the HTML element via the `data-controller` attribute. The value we set for this attribute let's us autoload the corresponding controller.

For example if I have a `<div data-controller="user">` in my HTML, Stimulus will load automatically the `controller/user_controller.js` file that will contains all the code that will respond to the element events. 

Here I assume that we follow the standard setup of Stimulus and store all the controllers in the `controller/` folder.

#### Actions
As in Rails a single controller can define multiple actions and each of them is a set of instruction to respond at specific events.

#### Targets
This is a single element of our HTML and we can specify a name to access it easily.

#### Values
Whit these we can read, write or observe changes in the attributes' values.