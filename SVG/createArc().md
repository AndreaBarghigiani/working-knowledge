
- [[#Horizontal left-to-right and vertical direction bottom|Horizontal left-to-right and vertical direction bottom]]
	- [[#When it happen in code?|Conditionals]]
- [[#Horizontal left-to-right and vertical direction top|Horizontal left-to-right and vertical direction top]]
	- [[#When it happen in code?|Conditionals]]
- [[#Horizontal right-to-left and vertical direction bottom|Horizontal right-to-left and vertical direction bottom]]
	- [[#When it happen in code?|Conditionals]]
- [[#Horizontal right-to-left and vertical direction top|Horizontal right-to-left and vertical direction top]]
	- [[#When it happen in code?|Conditionals]]
- [[#Vertical bottom-to-top and horizontal direction right|Vertical bottom-to-top and horizontal direction right]]
	- [[#When it happen in code?|Conditionals]]
- [[#Vertical bottom-to-top and horizontal direction left|Vertical bottom-to-top and horizontal direction left]]
	- [[#When it happen in code?|Conditionals]]
- [[#Vertical top-to-bottom and horizontal direction right|Vertical top-to-bottom and horizontal direction right]]
	- [[#When it happen in code?|Conditionals]]
- [[#Vertical top-to-bottom and horizontal direction left|Vertical top-to-bottom and horizontal direction left]]
	- [[#When it happen in code?|Conditionals]]

Keeping this file to document the thought process to improve the function.

The `createArc` fn is used to create the arc of a path drawn by an SVG.

## Common concept
Here I am going to write step by step how to draw the arc in order to simplify the conditionals.

### Horizontal left-to-right and vertical direction bottom
![[left-right-bottom.png]]
**Values to make the arc**:
- `sweep` - *1*
- `x` - *positive*
- `y` - *positive*

Example: `a 10 10 0 0 1 10 10`

#### When it happen in code?
- calculated current `H` is **minor** than next `H` and `v` is *positive* (next not calculated `H` can be *'close'* too)
- 
- both `h` and `v` are *positive*


### Horizontal left-to-right and vertical direction top
![[left-right-top.png]]
**Values to make the arc**:
- `sweep` - *0*
- `x` - *positive*
- `y` - *negative*

Example: `a 10 10 0 0 0 10 -10`
#### When it happen in code?
- current `H` is **minor** than next `H` and `v` is *negative*
- `h` is *positive* and `v` is *negative*

### Horizontal right-to-left and vertical direction bottom
![[right-left-bottom.png]]
**Values to make the arc**:
- `sweep` - *0*
- `x` - *negative*
- `y` - *positive*

Example: `a 10 10 0 0 0 -10 10`
#### When it happen in code?
- current `H` is **greater** than next `H` and `v` is *positive* (next not calculated `H` can be *'start'* too)
- `h` is *negative* `v` is *positive*
### Horizontal right-to-left and vertical direction top
![[right-left-top.png]]
**Values to make the arc**:
- `sweep` - *1*
- `x` - *negative*
- `y` - *negative*

Example: `a 10 10 0 0 1 -10 -10`
#### When it happen in code?
- current `H` is **greater** than next `H` and `v` is *negative*
- `h` is *negative* `v` is *negative*
### Vertical bottom-to-top and horizontal direction right
![[bottom-top-right.png]]
**Values to make the arc**:
- `sweep` - *1*
- `x` - *positive*
- `y` - *negative*

Example: `a 10 10 0 0 1 10 -10`
```
//Abs
H670a 10 10 0 0 1 10 -10
v-40a 10 10 0 0 1 10 -10 // This one I care about
H790a 10 10 0 0 1 10 -10

//Rel
h-88a 10 10 0 0 1 -10 -10
v-5a 10 10 0 0 1 10 -10 // This one I care about
h39a 10 10 0 0 1 10 -10
```
#### When it happen in code?
- `v` is *negative* and previous `H` is **minor** than next `H`
- `v` is *negative* and previous `h` (*negative*) is **minor** than next `h` (*positive*)
### Vertical bottom-to-top and horizontal direction left
![[bottom-top-left.png]]
**Values to make the arc**:
- `sweep` - *1*
- `x` - *positive*
- `y` - *negative*

Example: `a 10 10 0 0 0 -10 -10`
```
// Abs
H80a 10 10 0 0 1 10 -10 
v-39a 10 10 0 0 0 -10 -10 // This one I care about
H30a 10 10 0 0 1 -10 -10

// Rel
h-88a 10 10 0 0 1 -10 -10
v-2a 10 10 0 0 0 -10 -10 // This one I care about
h-137a 10 10 0 0 1 -10 -10
```
#### When it happen in code?
- `v` is *negative* and previous `H` is **greater** than next `H`
- `v` is *negative* and previous `h` is **minor** than next and both are *negative*
### Vertical top-to-bottom and horizontal direction right
![[top-bottom-right.png]]
**Values to make the arc**:
- `sweep` - *0*
- `x` - *positive*
- `y` - *positive*

Example: `a 10 10 0 0 0 10 10`
```
//Abs
H910a 10 10 0 0 1 10 10
v39a 10 10 0 0 0 10 10
H960a 10 10 0 0 1 10 10

//Rel
h627a 10 10 0 0 1 10 10
v7.82a 10 10 0 0 0 10 10 // This one I care about
h284a 10 10 0 0 1 10 10
```
#### When it happen in code?
- `v` is *positive* and previous `h` is **greater** than next that's *positive*
- `v` is *positive* and previous `H` is **minor** than next
### Vertical top-to-bottom and horizontal direction left
![[top-bottom-left.png]]
**Values to make the arc**:
- `sweep` - *1*
- `x` - *negative*
- `y` - *positive*

Example: `a 10 10 0 0 1 -10 10`
```
//Abs
H990a 10 10 0 0 1 10 10
v395a 10 10 0 0 1 -10 10 // This one I care about
H930a 10 10 0 0 0 -10 10

//Rel
h960a 10 10 0 0 1 10 10
v98a 10 10 0 0 1 -10 10 // This one I care about
h-29a 10 10 0 0 0 -10 10
```
#### When it happen in code?
- `v` is *positive* and previous `H` is **greater** than next
- `v` is *positive* and previous `h` is **greater** than next that's *negative*