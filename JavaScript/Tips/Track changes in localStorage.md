[[localStorage]] is a great thing that we have in order to store some data inside the browser of our user.

Sometime you need to track the changes a specific key will have because keeping track of the changes could be difficult based on the app size you're building. 

In these cases you can leverage the classic [[addEventListener]] passing the `storage` event, inside it's callback you can access the specific data that gets changed and [[console.trace|trace]] it down to discover which function has updated the data stored in it.

```js
window.addEventListener('storage', () => {
  // When local storage changes, dump the list to
  // the console.
  console.log(JSON.parse(window.localStorage.getItem('sampleList')));
});
```

**Remember** that this will track changes that came from different windows (aka site opened in a new tab) and that the tracing of it isn't useful since in this case will be track the `console.log` that is showing the data and not the function that has changed them.