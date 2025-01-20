### `reduce`
`reduce` is one of my favorite methods, it lets you do **a lot** with your array but its inner workings sometimes can push people off. The most basic example of it is a loop that sums up all number in an array: ^224568
```js
const numbers = [1,2,3,4]

const sum = numbers.reduce((acc, cur) => acc + cur, 0)
```

Basically speaking, `reduce` takes two arguments:
- a **function** that will be called each loop
- a **start value** that will be used in for the first iteration as the `acc` variable