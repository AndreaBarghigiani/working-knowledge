The `IntersectionObserver` allow us to asynchronously check if the target element is intersecting with an ancestor element or the top-level viewport.

This API can be helpful to:
* lazy load images 
* implement "infinite scrolling"
* basically decide to run or not fns based on visibility of an element

## Concept and usage
With the `IntersectionObserver` you can configure a callback that is called when occurs one of the following:
* a target elements intercept a root element (could be the viewport or a specific element)
* the first time an observer observe a specific element

You can specify the amount of intersection via the **intersection ratio** that is a numeric value between `0.0` and `1.0`. 

### Constructor options
`IntersectionObserver` accept three options:
* `root` - this is the root element that will be the one that the target element will intersect, defaults to browser viewport
* `rootMargin` - behaves similarly to `margin` CSS prop and serves to grow or shrink of the root element bounding box
* `threshold` - can be a single number or an array that indicates the percentage of target's visibility that will fire the callback

```js
let options = {
  root: document.querySelector('#scrollArea'),
  rootMargin: '0px',
  threshold: 1.0
}

let observer = new IntersectionObserver(callback, options);
```

### Target an element to be observed
Once you create the foundation to observe the visibility of the target it's time to select the element that needs to be observed. To do it you have to pass the target to the `observe` method.

```js
let target = document.querySelector('#listItem');
observer.observe(target);
```

When the target will met the `threshold` defined the `callback` will be fired.

The `callback` will receive a list of each target that is reporting a change in its intersection status.