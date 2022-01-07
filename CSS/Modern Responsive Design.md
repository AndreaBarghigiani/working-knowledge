## Grow a container until a point
Generally speaking we usually set a responsive container like so:
```css
.container{
	max-width: 1000px;
	width: 100%;
}
```
This tells the browser to stretch the `.container` until `1000px` but we can do it in one-liner:
```css
.container{
	width: min(100%, 1000px);
}
```
## Center horizontally
Once your `.container` has a set `width` in most layouts it needs also to be centered, we can do it by setting specific margins to the left and right:
```css
.container{
	margin-left: auto;
	margin-right: auto;
}
```
Or commonly using the shorthand `margin`:
```css
.container{
	margin: 0 auto; /* or simply auto */
}
```
But today we've a better way to do so:
```css
.container{
	margin-inline: auto;
}
```
This tells the browser to consider the value only for the left and right sides ignoring top and bottom.
## Dynamically add space to `padding` and `margin`
It is logic to dedicate more real estate to our elements on a bigger viewport but that's not doable with standard viewport units, that's because they do not have any kind of break point and the value keeps changing.

In order to better handle this situation we can make use of the `max()` function that, opposite of `min()`, tells the browser to select the maximum value applicable:
```css
.element{
	padding: max(3vh, 1rem) 1.5rem;
	margin-top: max(2vh, .5rem);
}
```
## Fluid typography with `clamp`
Up until now if you wanted to create fluid typography you had to import some sort JS library or apply some complex [CSS `calc`](https://css-tricks.com/snippets/css/fluid-typography/).

Nowadays we can reach the same result with `clamp`:
```css
.element{
	font-size: clamp(1rem, 10vw, 2rem);
}
```

You can use [`clamp`](https://developer.mozilla.org/en-US/docs/Web/CSS/clamp()) on many properties, `font-size` it's just the one that makes more sense as an example.
#### Resources
- [3 modern CSS techniques](https://www.youtube.com/watch?v=VsNAuGkCpQU)