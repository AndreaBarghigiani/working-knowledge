Before ES6 if you had to swap two elements in an Array you had to create a temp variable:
```js
const tmp = a[4]
a[4] = a[3]
a[3] = tmp
```
But now thanks of [[destructuring]] you can simply swap them like so:
```js
[a[3], a[4]] = [a[4], a[3]]
```