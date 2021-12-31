
  In many cases you want to set a default value for a variable only if the value we try to assing is `null` or `undefined`.

You can think that using the logical OR (`||`) operator you're just fine but that's not true. Check out this example:
```js
const user = {
	name: 'Andrea',
	admin: false
}

const isAdmin = user.admin || true // true
```
In this case `isAdmin` will be true because the OR operator will check the truthyness of the left side value and since it is `false` will assing the right value to the `isAdmin` variable.

If you are specifically interested to know if the value is set or not, does not matter it's value, in this case you should use the Nullish Coalescing operator:
```js
const user = {
	name: 'Andrea',
	admin: false
}

const isAdmin = user.admin ?? true // false
```
This works because the property `admin` of my object **is set** to `false` and this operator will ignore the left side value **only if** it is `null` or `undefined`.

Exactly for the reason above the Nullish Coalescing operator is extremely useful when used in conjunction to the optional chaining operator. With optional chaining if we don't find the property the interpreter will just return `undefined` instead of trowing an error:
```js
const user = {
	name: 'Andrea'
}

const isAdmin = notDefinedUser?.admin ?? true // true
```