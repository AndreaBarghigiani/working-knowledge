This fn helps you to find an object that's nested deep down a tree:
```js
const findNestedObject = (object = {}, keyToMatch = "", valueToMatch = "") => {
  if (isObject(object)) {
    const entries = Object.entries(object);

    for (let i = 0; i < entries.length; i += 1) {
      const [objectKey, objectValue] = entries[i];

      if (objectKey === keyToMatch && objectValue === valueToMatch) {
        return object;
      }

      if (isObject(objectValue)) {
        const child = findNestedObject(objectValue, keyToMatch, valueToMatch);

        if (child !== null) {
          return child;
        }
      }
    }
  }

  return null;
};
```