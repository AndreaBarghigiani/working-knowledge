One of the best things about [[Intro|SVG]] is that they are build just using a specific syntax, that means you can edit them and change with simple code.

SVG offers many kind of elements you can experiment with: `rect`, `circle`, `ellipse` to write a few. But here our focus will be the `path` element, specifically it's `d` attribute.

The `path` element is really useful because allow us to draw any kind of shape, and it does this by combining multiple straight or curved lines but in order to define those lines we will always use the `d` attribute.

Inside this attribute we define a series of commands and parameters that help use in drawing the shape we want. Each command is instantiated with a letter but before to go further let's talk about the elephant in the room.

The commands come in two variants:
* **uppercase letter** - let us define the absolute coordinates on the page
* **lowercase letter** - with them we use relative coordinates

Remember that the coordinates are **always unitless**.

# Line commands
While creating SVG we have five line commands, the first one is the `M` (or `m` if you prefer to be relative) that allow use to move the cursor without drawing anything on it. This is pretty useful if we need to create different lines that are not connected.

Little introduction an all the commands (and their syntax):
* `M x y` (or `m dx dy`) *Move To*- takes two parameters and that defines the point were our cursor will be  ^f58ce7
* `L x y` (or `l dx dy`) *Line To* - draw the line from where we are (specified by `M`) to where we want to go (specified by the coordinates)
* `H x` (or `h dx`) *Horizontal Line To* - draw an horizontal line from where we are to where we want to go in the horizontal axes
* `V y` (or `v dy`) *Vertical Line To* - draw a vertical line from where we are to where we want to go in the vertical axes
* `Z` (or `z`) *Close Path* - we place this command at the end of our declaration and help us because tells the SVG to close the path without care of where we are

![[svg-line-commands.png]]

So with this knowledge at our disposal let's see the different ways to create a square, notice that all the following example will be placed in this SVG so I'll just insert the values for the `d` attribute:
```svg
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
	<path d="" fill="transparent" stroke="black" />
</svg>
```

### Using the `L` *(Line To)* command
```
M 10 10 
L 90 10 
L 90 90 
L 10 90 
L 10 10
```
### Using the `l` *(Line To)* command (relative version)
```
M 10 10 
l 80 0 
l 0 80 
l -80 0
l 0 -80
```
Since we're using relative coordinates we can substract from our point.
### Using the `H` *(Horizontal Line To)* and `V` *(Vertical Line To)* commands
```
M 10 10 
H 90 
V 90 
H 10
V 10
```
### Using the `h` *(Horizontal Line To)* and `v` *(Vertical Line To)* commands (relative version)
```
M 10 10 
h 80 
v 80 
h -80
v -80
```
### Using the `Z` *(Close Path)* command with `H` and `V`
```
M 10 10 
H 90 
V 90 
H 10
Z
```
### Using the `Z` *(Close Path)* command with `h` and `v` (relative version)
```
M 10 10 
h 80 
v 80 
h -80
z
```
Based on my experiments you can use `Z` and `z` interchangeably.

# Curve commands
Those are some of the most complex commands because they involve a lot of math, so bare with me and be sure to check the [proper section on MDN](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths#curve_commands) if you want to dive deeper.

* `C x1 y1, x2 y2, x y` (or `c dx1 dy1, dx2 dy2, dx dy`) - Bézier Curves are considered the most complex to create because they require the end point of the curve (the last `x y`) and the two control points that define the curve. `x1 y1` defines the curve for the starting point while `x2 y2` defines the curve for the end point
* `S x2 y2, x y` (of `s dx2 dy2, dx dy`) - simplified version of cubic Bézier curve where we assume that the two control points are symmetrical. If you chain this commands the computer assumes that the first control point is a reflection of the one used previously
* `Q x1 y1, x y` (or `q dx1 dy1, dx dy`) - this is the quadratic curve where the control point for both points just match
* `T x y` (or `t dx dy`) - behaves like `S` but reflects the quadratic curves
* `A` (or `a`) - the syntax for is pretty long so watch out for the next section, basically let us define arcs

![[svg-curve-commands.png]]

## Arc

^7c4dbd

At any moment you can draw 4 different arcs that connects two points, you can chose to draw a **large** or a **small** arc (with the `large-arc-flag`) and you can decide to draw a **clockwise turning** arc or a **counterclockwise turning** arc (with the `sweep-flag`).

This is an image taken from [MDN article](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths):
![[svgarcs_flags.png]]

Here's the full syntax:
`A rx ry angle large-arc-flag sweep-flag x y` that uses absolute coordinates and
`a rx ry angle large-arc-flag sweep-flag dx dy` that uses the relative ones.
* `rx` and `ry` defines the radius of the arc, you have two separate values because it allows you to draw ellipses as well
* `angle` if you have an ellipse this value is able to change the rotation of your element
* `large-arc-flag` is a boolean value, means it can be turned off (`0`) or on (`1`) and allows us to draw a small or a large arc
* `sweep-flag` is a boolean value, means it can be turned off (`0`) or on (`1`) and allows us to draw the arc with a counterclockwise or clockwise turn
* `x y` or `dx dy` define the center of the arc