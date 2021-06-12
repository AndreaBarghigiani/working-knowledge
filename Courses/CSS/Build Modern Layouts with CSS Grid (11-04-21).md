We define a container as a grid with `display: grid` and we can handle the numbers of row and columns respectively with `grid-template-rows` and `grid-template-columns`.
```css
.container{
	display: grid;
	grid-template-columns: 100px 100px;
	grid-template-rows: 75px 75px;
}
```
The code above will create a 2x2 grid with columns of `100px` and rows of `75px`.

We can also add space around our grid by adding a gap that can be specified for columns and rows:
```css
.container{
	// Same code as before ...
	grid-column-gap: 10px;
	grid-row-gap: 20px;
}
```
`grid-gap: [row] [columns]` is the shorter version of using the two separately.

Working with the grid inspector can easily became a pain, by default our browsers will render a numeric index for each start/end point of our grid areas, the line numbers.

But we can do a better job, we can give names to the areas we create with `grid-template-areas`, beside help our debug allows us to easily give an order to the elements of our page by respecting its structure.
```css
.container {
  /* Other code... */
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 5fr 1fr;
  grid-template-areas: 
  "header header"
  "nav content"
  "footer footer";
}
```
Doing so our browser is not able to decide which elements select and place, we need to help it by connecting the area name to the specific element. `grid-area` is the specific rule that allows us to do that:
```css
.header{
	grid-area: header;
}
.nav{
	grid-area: nav;
}
.content{
	grid-area: content;
}
.footer{
	grid-area: footer;
}
```
Here I am using the [same classes as in the lesson](https://github.com/hiroko/egghead-lesson/blob/master/cssgrid-course/06-lesson/after/index.html) but the concept should be simple: you can give an area name to an element in the page and it does not matter where is positioned in your HTML, as long as shares the same `display: grid` container will follow the order defined in `grid-template-areas`. 

Defining `grid-template-columns` or `grid-template-rows` can be a repetitive task and as we know **computer are great at repeating stuff**, for this reason CSS gave us the `repeat()` fn that we can use to define more values easily:
```css
.container {
  /* OTher code... */
  grid-template-columns: repeat( 5, 1fr );
  grid-template-rows: repeat( 2, 1fr );
}
```
The syntax is pretty basic, the first parameter is the number of times you want to repeat the *unit value* that you declare as second.

The above is the same as write:
```css
.container {
  /* OTher code... */
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr;
}
```
The second value that we pass to `repeat` can also be a multiple definition, in this case we will repeat all of them:
```css
.container {
  /* OTher code... */
  grid-template-columns: repeat(2, 1fr 2fr);
  /* Is the same as we write */
  grid-template-columns: 1fr 2fr 1fr 2fr;
}
```
### Column and row units
* `%` - when we use percentages we need to consider also the gap, this value **will not** account for the size of the gap
* `px` - not the best idea in the world, makes not responsive the grid
* `fr` - they seems to be our best candidates, they account the gap size used and are responsive by nature

We can mix all kind of values we want.
### Align items
To align vertically (the column axes) a content inside of a grid we can use `align-items` that will behave in a similar way that the ones from Flexbox. If we are looking to align horizontally (the row axes) we can use `justify-items`.
### Controlling `min` and `max` in a grid
With the single `minmax(min, max)` fn we can define the dimension of the single column or row specifying a `min` size and the `max` one. Let's mix it with the `repeat` fn just to have some fun:
```css
.container {
  /* OTher code... */
  grid-template-columns: repeat(5, minmax(200px, 1fr));
}
```
In a single declaration we have created a columns template of 5 item, each column will have `min-width: 200px` and `max-width: 1fr`.

This approach create though an unresponsive layout, if the screen width is not enough to have 5 columns of `200px` each the browser will start to create an horizontal scroolbar.

If we want to still use `repeat` and we have a fixed size in place, then we can let the browser decide for us the best number of items with the special `auto-fit` keyword.
```css
.container {
  /* OTher code... */
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}
```
