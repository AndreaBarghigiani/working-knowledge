Checks if the passed argument is an object or not.
```js
const isObject = (value) => {
  return !!(value && typeof value === "object" && !Array.isArray(value));
};
```