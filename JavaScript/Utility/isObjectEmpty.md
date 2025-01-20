Checks if an object contains any properties to define whether it is empty.
It uses the other [[isObject]] utility function to exit early if the value passed as an argument is not an object.
```js
const isObjectEmpty = (obj) => {
  if (!isObject(obj)) return false;
  for (const prop in obj) {
    if (obj.hasOwnProperty(prop)) {
      return false;
    }
  }

  return true;
};
```