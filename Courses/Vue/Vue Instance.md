The middle man between our business logic and the DOM and has many gotchas. As per any object, the instance of Vue can be created with `new Vue`.

## Use multiple instance
It is possible to use multiple instances of Vue in a page, we just need to pass a different selector inside the `el`.

Since are separate instances all the code we write in one of them **will not be accessible** from the other one.

## Access instance from outside
We can store our instance simply by assing it to a variable, this let me also change the data from an instance from a different one.
```js
let vm1 = new Vue({
	el: 'app1',
	data: {
		title: 'Default title for 1'
	}
})

let vm2 = new Vue({
	el: 'app2',
	methods:{
		onChange(){
			vm1.title = 'Changed!' // Changing the data of different instance
		}
	}
})
```
We can also access the instance from outside all Vue instances, we just need to refer the variable that stores it:
```js
setTimeout(function(){
	vm1.title = 'Changed from timeout'
}, 2000)
```
## Data and methods are "cached"
If we inspect the instance of Vue we notice that all the properties we defined inside are in a different place, the structure that we set inside the instantiation of the object are used inside the Vue constructor to let it do its magic.

