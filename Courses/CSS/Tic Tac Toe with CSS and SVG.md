> Notes from [Tic Tac Toe with CSS and SVG](https://egghead.io/courses/tic-tac-toe-with-css-and-svg-be02) course on Egghead. It's a **free** course 😉

The base system is built with Pug to generate 9 [[checkbox]] for the `x` and other 9 for the `o`, we do this because in the project we do not use [[JavaScript]] at all so we need a way to tell our browser what is clicked.

Then we will build the `.board` with the same loop used and we create the labels that once clicked will check the appropriate `checkbox`. Here's the full code at the end of the lesson:
```pug
form.game
  - for (let i = 0; i < 9; i++)
    input(type='checkbox' id=`x-${i}`)
    input(type='checkbox' id=`o-${i}`)
  .board
    - for (let i = 0; i < 9; i++)
      .board__cell
        label(for=`x-${i}`)= `Cross ${i}`
        label(for=`o-${i}`)= `Naught ${i}`
```
That is able to generate the following HTML:
```html

<form class="game">
  <input type="checkbox" id="x-0"/>
  <input type="checkbox" id="o-0"/>
  <input type="checkbox" id="x-1"/>
  <input type="checkbox" id="o-1"/>
  <input type="checkbox" id="x-2"/>
  <input type="checkbox" id="o-2"/>
  <input type="checkbox" id="x-3"/>
  <input type="checkbox" id="o-3"/>
  <input type="checkbox" id="x-4"/>
  <input type="checkbox" id="o-4"/>
  <input type="checkbox" id="x-5"/>
  <input type="checkbox" id="o-5"/>
  <input type="checkbox" id="x-6"/>
  <input type="checkbox" id="o-6"/>
  <input type="checkbox" id="x-7"/>
  <input type="checkbox" id="o-7"/>
  <input type="checkbox" id="x-8"/>
  <input type="checkbox" id="o-8"/>
  <div class="board">
    <div class="board__cell">
      <label for="x-0">Cross 0</label>
      <label for="o-0">Naught 0</label>
    </div>
    <div class="board__cell">
      <label for="x-1">Cross 1</label>
      <label for="o-1">Naught 1</label>
    </div>
    <div class="board__cell">
      <label for="x-2">Cross 2</label>
      <label for="o-2">Naught 2</label>
    </div>
    <div class="board__cell">
      <label for="x-3">Cross 3</label>
      <label for="o-3">Naught 3</label>
    </div>
    <div class="board__cell">
      <label for="x-4">Cross 4</label>
      <label for="o-4">Naught 4</label>
    </div>
    <div class="board__cell">
      <label for="x-5">Cross 5</label>
      <label for="o-5">Naught 5</label>
    </div>
    <div class="board__cell">
      <label for="x-6">Cross 6</label>
      <label for="o-6">Naught 6</label>
    </div>
    <div class="board__cell">
      <label for="x-7">Cross 7</label>
      <label for="o-7">Naught 7</label>
    </div>
    <div class="board__cell">
      <label for="x-8">Cross 8</label>
      <label for="o-8">Naught 8</label>
    </div>
  </div>
</form>
```
Remember to give the user a way to [[Reset a form|reset the form]], it's just simple HTML.

## Create the board
To create the board we just use a simple [[CSS grid]] that will help us to create the 3x3 board that we need. Nothing special here:
```css
:root {
  --size: 50vmin;
}

.board {
  height: var(--size);
  width: var(--size);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  place-items: center;
}
```
We use the power of the [[CSS custom variables]] to define the size of our cells. Also note:
* we define `columns` and `rows` but since they are the same the latter is totally optional
* in this case we use `place-items` to [[center the content of the box]], is the same as writing the `center` for `align-items` and `justify-content`

## Toggle Cross and Naught
Since each cell has both the `x` and `o` value, we need to find a way to alternate what the user will click. This is one of the trickiest part but we can achieve with just CSS with the help of the checked pseudo selector with the general sibling combinator:
```css
:checked ~ .board .board__cell label:nth-of-type(odd){
	display: block;
}

:checked ~ .board .board__cell label:nth-of-type(even){
	display: none;
}

/* To see what we're doing */
label:nth-of-type(odd) {
  color: #f00;
}
label:nth-of-type(even) {
  color: #008000;
}
```
This code will help us for the first click but we need to handle all the following clicks so the selector will grow exponentially, be sure to [check the video]() to fully understand what is happening:
```css
:checked ~ .board .board__cell label:nth-of-type(odd),
:checked ~ :checked ~ .board .board__cell label:nth-of-type(even),
:checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(odd),
:checked ~ :checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(even),
:checked ~ :checked ~ :checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(odd),
:checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(even),
:checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(odd),
:checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(even) {
  display: block;
}
:checked ~ .board .board__cell label:nth-of-type(even),
:checked ~ :checked ~ .board .board__cell label:nth-of-type(odd),
:checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(even),
:checked ~ :checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(odd),
:checked ~ :checked ~ :checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(even),
:checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(odd),
:checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(even),
:checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ :checked ~ .board .board__cell label:nth-of-type(odd) {
  display: none;
}
label:nth-of-type(odd) {
  color: #f00;
}
label:nth-of-type(even) {
  color: #008000;
}
```
## Stop user from using the same cell
In order to do so we start by creating a series of elements that we will use to overlay the cell once clicked making impossible to click again the `label` underneath.
```pug
- for (let i = 0; i < 9; i++)
    - const x = i % 3
    - const y = Math.floor(i / 3)
    input(type='checkbox' id=`x-${i}`)
    .x(style=`--x: ${x}; --y: ${y}`)
    input(type='checkbox' id=`o-${i}`)
    .o(style=`--x: ${x}; --y: ${y}`)
```
So while we're creating the cell in Pug, we also generate the [[custom property]] and the values that we need to identify the position. Also notice that we're adding the classes `.x` and `.o` so later will be easy to change the background based on the selection.
## Use SVG to create the lines 
Now the logic is in place, we need to create the board for it to play. Instead of creating it with strange `border`s all over the place we will use SVG:
```pug
.board
    - for (let l = 0; l < 4; l++)
      - const rotate = l % 2
      - const shift = 2 % (l + 1) ? 1 : -1
      svg.board__line(viewBox="0 0 10 300" style=`--rotate: ${rotate}; --shift: ${shift};`)
        path(d="M 5 5 L 5 295" stroke-width="10" stroke-linecap="round" stroke="black")
```
Inside the `.board` element we create a new loop that will help us creating the 4 lines for our game. Note how we generate the values for `--rotate` and `--shift` based on the loop. This is helpful to know where to position the lines with the following:
```css
.board__line {
  position: absolute;
  height: var(--size);
  width: calc(var(--size) * 0.05);
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%) rotate(calc(var(--rotate, 0) * -90deg)) translateX(calc(var(--shift, 0) * ((var(--size) / 3) * 0.5)));
}
```
The most important part of this block of code is the `transform`, let's try to explain it:
* we use `translate(-50%, -50%)` to perfect center the line in `absolute` position
* with `rotate(calc(var(--rotate, 0) * -90deg))` we decide if we need to rotate the line and make horizontal
* `translateX(calc(var(--shift, 0) * ((var(--size) / 3) * 0.5)))` it seems the most complex but basically we're telling how much we need to move the line based on the `--shift` value (the `rotate` will decide if we move vertically or horizontally)

> From here I just keep watching because it is just sick, do the same and be amazed from the complexity of the selectors 😂 