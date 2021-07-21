The base idea is to build a system to create a frame that wraps the page.
All the original values are treated as if they are declared in `%` so is going to be easier for use to declare the initial value.

For example, if I am passing `80` to the `h` command I want to draw an horizontal line that is `80%` of the entire window. In order to do so the first thing I am doing is to calculate the size of the window with:

```js
const vw = Math.max(
  document.documentElement.clientWidth || 0,
  window.innerWidth || 0
);

const vh = Math.max(
  document.documentElement.clientHeight || 0,
  window.innerHeight || 0
);
```
**First issue:** the `window.innerWidth` or `window.innerHeight` do not account for the scrollbar size so depending on the browser used or the scroll implementation **this could be ignored**.

Now that I have the sizes I apply those values to the `viewBox`, `width` and `height` attributes of the selected SVG:
```js
svg.setAttribute("width", `${vw}px`);
svg.setAttribute("height", `${vh}px`);
svg.setAttribute("viewBox", `0 0 ${vw} ${vh}`);
```

And I use the same values to translate the `80%` to the pixels of my `viewBox`:
```js
// Definition
function percToPixel(val, base) {
  return val * base / 100;
}

// Example use
const pixelH = percToPixel(80, vw); // Because in the example I am using the 'h' command
```
Theoretically this is correct, I am getting the `px` value for an horizontal line that's declared as `80%` of the entire viewport **BUT** we do not account for the starting point of our path.

The previous calculation should work fine if I start to dray at `M 0 0`, but as soon as I move the start from the origin I should account for that.
# Attempts to use `M` values as frame width
## Draw a square frame
### Goal
I want to draw a square passing just the full size and use the starting point defined by the `M` as margin for the shape. So if I pass the following with a `M 30 30` I should have a margin of around `30px`:
```js
const square = [
	['h', 100],
	['v', 100],
	['h', -100],
	['v', -100]
]
```

#### How we draw the shape
To clarify here are the steps that the SVG will take to draw this kind of shape:
1. move the *'pen'* at `30 30`,
2. the `percToPixel()` will translate `100` to whatever value needed and draw the first horizontal line
3. now it's time for our first vertical line of `100`
4. we now draw the `-100` horizontal line
5. time for the `-100` vertical line

Let's add a simple example with numbers confronting with the way we do until now to make obvious the problem, the viewport is `1200x1200` with an `M 100 100` as starting point:
1. move the *'pen'* at `100 100`
2. `['h', 100]` will be `1000` 
3. `['v', 100]` will be `1000` 
4. `['h', -100]` will be `-1000` 
5. `['v', -100]` will be `-1000`

![[square-svg.png]]

### Solution
In order to reach this result all I need is to change the `base` value that I pass to `percToPixel()` and account for the values set for the `M` command. To do so I remove **two times** the values that are present in it because I also need to reflect the same value to the right (or the bottom) or our window.

So in order to calculate the right size I can't pass `vw` or `vh` anymore but I have to:
```js
const pixelH(100, vw - 30 * 2);
const pixelV(100, vh - 30 * 2);
```

## Draw a rounded corner square frame
### Goal
I want to draw the same square but this time with rounded corner

#### How we draw the shape
To clarify here are the steps that the SVG will take to draw this kind of shape:
1. move the *'pen'* at `30 30`,
2. the `percToPixel()` will translate `100` to whatever value needed and draw the first horizontal line
3. the `createArc()` will create  the arc and **will be add** `10` for `x` and `10` for `y`
4. now it's time for our first vertical line of `100`
5. at the end of the vertical line the `createArc()` **subtract** `10` for `x` and **adds** `10` for `y`
7. we now draw the `-100` horizontal line
8. now `createArc()`  **subtract** `10` for `x` and **subtract** `10` for `y`
9. time for the `-100` vertical line
10. `createArc()` **add** `10` for `x` and **subtract** `10` for `y`

Let's add a simple example with numbers confronting with the way we do until now to make obvious the problem, the viewport is `1200x1200` with an `M 100 100` as starting point:
1. move the *'pen'* at `100 100`
2. `['h', 100]` will be `1000` (absolute end point `1100`)
3. add an `arc` and the new starting point for the next line will be `1200 200`
4. `['v', 100]` will be `1000` (absolute end point `1200`)
5. add an `arc`  and the new starting point for the next line will be `1100 1300`
6. `['h', -100]` will be `-1000` (absolute end point `100`)
7. add an `arc`  and the new starting point for the next line will be `0 1200`
8. `['v', -100]` will be `-1000` (absolute end point `200`)
9. add an `arc`  and the new starting point for the next line will be `100 100`

Now the path is closed **but** the issue is that we moved increase the shape:
![[svg-shape-rounded-wrong.png]]

The original shape that we wanted, the light gray square, now has grown `100` points to the right, `100` to the left and `200` at the bottom. Instead what we wanted to achieve is the following:
![[svg-shape-rounded-good.png]]

Basically all the lines needs to account for the arc radius **and** we need to move the horizontal start point by the arc radius.

### Solution
I will use the [[How to draw a `path` with code#^7c4dbd|`arc` command]] with a radius of `100` for both axes and now **I have to account for it's dimensions too**. Following this example here is the syntax of the arc that will be drawn for the top right:
```
a 100 100 0 0 1 100 100
```

We created the `createArc()` fn so we can delegate to it the *'direction'* of the arc.

> For the moment the radius will **not** be passed to the `percToPixel()` and will be used as is.

Now that we know how to draw an arc in our path let's tackle the previous problems.

#### 1. Move the starting point by arc radius
That's pretty easy, when we declare the [[How to draw a `path` with code#^f58ce7|Move To]] command we just need to add the radius to the horizontal point:
```js
const arcRad = 100;

// Instead of 
path += `M${hStart} ${vStart}`;

// account for arc radius
path += `M${hStart + arcRad} ${vStart}`;
```

Now we have the correct starting point but we need to set the proper line width, otherwise our SVG will be drawn out of the `viewBox`.

#### 2. Set the correct line length
Even this one is pretty easy, when we create a line we just need to subtract twice the radius:
```js
// Add an horizontal line 
path += 'h' + (percToPixel(100, 1200 - 100 * 2) - 100 * 2 );
```
Remember that we are working with following values:
* `0 0 1200 1200` this is the `viewBox` sizes
* `M 100 100`, we work with the original values, don't care about the previously changes
* `100` is the radius of the arc

This is not all though because we work also with negative values so while we're looping the array of lines we can better handle the calculation:
```js
coords.forEach( (coord) => {
	const axis = coord[0] === "h" ? vw - hStart * 2 : vh - vStart * 2;
    const operation = coord[1] > 0 ? -1 : 1;
    const considerArc = arcRad * 2 * operation;
	
	// Add line
    path += coord[0] + (percToPixel(coord[1], axis) + considerArc);
	
	// And add the arc to the path...
})
```
With this approach we're able to get the same (correct) shape that we wanted.
## Draw a custom square shape
### Goal
Now, let's put aside the arc calculations for a moment and try to build this shape:
![[svg-custom-square-shape.png]]

So based on the shape we can thing that the array of lines could be this:
```js
const shape = [
	['h', 100],
	['v', 20],
	['h', -10],
	['v', 20],
	['h', 10],
	['v', 60],
	['h', -100],
	['v', -100]
]
```
### Solution
Well seems that we already have done a good job, our shape is drawn without any problems. Even trying a more customized shape the ending result seems to play out just fine.
```js
const oddCampShape = [
  ["h", 90],
  ["v", 10],
  ["h", 10],
  ["v", 80],
  ["h", -10],
  ["v", 10],
  ["h", -80],
  ["v", -10],
  ["h", -5],
  ["v", -65],
  ["h", -5],
  ["v", -25],
];
```
## Draw a custom curved shape
### Goal
Title explain everything.
### Solution
As for before, it is enough to change the radius for the value and everything play out just nice. Just be aware of the value of it because it can easily mess things up.
## Set a max width or height for a shape
So until now we've been able to draw all the shape we wants and the normal border respects the size of the starting point, but there is a single problem that now we need to solve: keep the size of the other elements fixed at different screens.
### Goal
I want to be able to say to the browser that a specific shape he's drawing could be at max a size that I declare.
![[svg-fixed-horizontal-size.png]]
For the moment let's focus on the horizontal lines considering that the vertical ones are less buggy on the responsive side.
#### Attempt 1 - Pass a third parameter to define max size
First idea that comes to mind is to pass a third parameter to state the max width that is allowed, I need though to find a way to keep track of the calculated value in order to account for next lines.
#### Attempt 2 ~~abandoned~~ - Only pass the desired size in string
> Here's the base idea was to calculate the point using the difference between the values (string or not) as a delimiter, turns out that since `h` is a relative value the result will be too.

Instead of telling how much a line must be, we could just pass the amount of space that we want left. Because we know the pixel value of the entire `viewBox` and we could approach this way.

So for example I could write something like:
```js
const points = [
	['h', "30"], // This must be 30px from the right
	['v', 50], // Integer, means calc the length in %
	['h', "close"], // Tell the SVG to draw until the end of the viewbox
	['v', 50], // We reach the bottom of the SVG
	['h', -100], // Integer, means calc the length in %
	['v', -100] // Integer, means calc the length in %
]
```

At this point all I have to do is a bit of math and use `H` instead of `h`. Similarly to `createArc()` I've created `createPath()` to handle the logic and extract it from the loop:
```js
function createPath(point) {
	let [command, val] = point;
	let calculated;
	const axis = command === "h" ? vw - hStart * 2 : vh - vStart * 2;
	const operation = val > 0 ? -1 : 1;
	const considerArc = arcRad * 2 * operation;

	if (typeof val === "string") {
		command = "H";
		switch (val) {
			case "close":
				calculated = vw - hStart;
				break;
			case "start":
				calculated = hStart;
				break;
			default:
				val = parseInt(val, 10);
				calculated = val < 0 ? Math.abs(val) : vw - val;
				calculated += considerArc;
				break;
		}
	} else {
		calculated = percToPixel(val, axis) + considerArc;
	}

	return command + calculated;
}
```
This works great for the square corners but not for arcs....

### Solution - Use `H` to define fixed points
While experimenting with `h` turns out that probably `H` is the definitive answer, once calculated it's value the point drawn will be **always at the same place** does not matter the frame size.

The base set of coordinates is now built like so:
```js
const oddCampShape = [
  ["H", 100],
  ["v", 5],
  ["H", 50],
  ["v", 5],
  ["H", "close"],
  ["v", 80],
  ["H", 100],
  ["v", 10],
  ["H", "start"],
  ["v", -100],
];
```
For `H` values can be an integer or a string. 
A *positive* value will define the amount of pixels that will be set **from the right margin** and the *negative* value will define the amount of pixel **from the left** (thinking to swap the logic but for now is good as is).

We also have two *string* values `close` and `start` and when used with `H` they allows me to reach the end of the frame respecting the original frame size.

> **Attention:** as for now (*304fedf5*) once you jump on the `H` train you have to use *'start'* or *'close'* and not rely anymore on the relative horizontal values

Mixing *integers* and *strings* involves more logic for the `createPath()` and `createArc()` and now they are transformed like so:
```js
// Both functions are closures!
function createPath(point) {
	let [command, val] = point;
	let calculated;
	const operation = val > 0 ? -1 : 1;
	let considerArc = arcRad * 2 * operation;

	if (command === "H") {
		prevHValue ??= val;
		considerArc = considerArc / 2;
		if (val === "close") {
			calculated = vw - hStart - considerArc;
		} else if (val === "start") {
			calculated = hStart + considerArc;
		} else if (val <= Math.abs(prevHValue)) {
			calculated =
				val < 0 ? Math.abs(val) + considerArc : vw - val + considerArc;
		} else if (prevHValue === "close") {
			calculated =
				val < 0 ? Math.abs(val) + considerArc : vw - val - considerArc;
		} else {
			calculated = val < 0 ? Math.abs(val) : vw - val;
		}
	} else {
		let axis = command === "h" ? vw - hStart * 2 : vh - vStart * 2;
		if (command === "h" && typeof prevHValue === "number") {
			axis -= prevHValue;
			considerArc *= -1;
		}
		calculated = percToPixel(val, axis) + considerArc;
	}
	return command + calculated;
}

function createArc(p, i) {
	const [command, val] = p;
	// Arc definitions
	let sweep = 0;
	let x = "";
	let y = "";

	// Logic booleans
	// Current
	const isPositive = val > 0;
	const isClose = val === "close";
	const isStart = val === "start";
	const isLast = i === points.length - 1;

	// Next
	const nextPoint =
		points[i + 1] &&
		(points[i + 1][1] > 0 ||
			points[i + 1][1] === "close" ||
			points[i + 1][1] === "start");

	if (command === "h" || command === "H") {
		// console.log({ prevHValue, val });
		if (nextPoint) {
			if (isPositive) {
				sweep = 1;
			} else if (isClose || isStart) {
				sweep = 1;
			} else {
				x = "-";
			}

			if (prevHValue === "close") {
				sweep = 0;
				x = "-";
			}
		} else {
			if (isPositive) {
				y = "-";
			} else {
				sweep = 1;
				x = y = "-";
			}
		}
		prevHValue = command === "H" ? val : prevHValue;
	} else {
		if (nextPoint) {
			if (!isPositive) {
				sweep = 1;
				y = "-";
				if (prevHValue < 0) {
					sweep = 0;
					x = "-";
				}
			} else if (prevHValue === "close") {
				sweep = 1;
				x = "-";
			} else if (points[i + 1][1] === "start") {
				sweep = 1;
				x = "-";
			}
		} else {
			if (isPositive) {
				sweep = 1;
				x = "-";
			} else {
				if (isLast) {
					sweep = 1;
					y = "-";
				} else {
					x = y = "-";
				}
			}
		}
	}

	return `a ${arcRad} ${arcRad} 0 0 ${sweep} ${x}${arcRad} ${y}${arcRad}`;
}
```
I am sure we can do better in writing the conditionals, especially for [[createArc()]], but is good enough for now.

Also please notice that we define this functions as closures because they rely on internal variables, especially `prevHValue`.

