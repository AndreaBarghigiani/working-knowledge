The **spead operator** is part of JavaScript since ES2015 and helps us "expand" an object or an array.

This makes it really easy to add items to this kind of type without mutating the original object.
```js
const data = {
	name: 'Andrea',
	last: 'Barghigiani',
};

const newData = {
	...data,
	age: 39, 
};
```
Now `data` and `newData` are two objects that have **different references** in memory. This is pretty useful in React where the state cannot be mutated to keep track of the changes and update the UI.

## Not only objects
As stated before, we can use this syntax even with [[array | arrays]], for example, it makes it incredibly easy (and performant), to add a new item at the beginning or end of the array:
```js
const arr = [2,3,4]
const addBefore = [1, ...arr]; // [1,2,3,4]
const addAfter = [...addBefore, 5] // [1,2,3,4,5]
```
> **Note:** in the last row, we spread `addBefore` and not the original `arr`, otherwise the result would've been: `[2,3,4,5]`.
  
### Deep dives