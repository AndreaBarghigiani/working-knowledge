A clousure is just a function that access values outside of it. (ref. [Egghead lesson](https://egghead.io/lessons/javascript-what-is-a-closure-in-javascript?pl=introduction-to-callbacks-broadcasters-and-listeners-5bd7)).

```js
let i = 0
let closure = () => {
  console.log(i++)
}
```
The above is a simple example of a closure because the declared function access a value that has been defined in the upper scope and is also able to change its value.
> Closure make it possible for a function to have *"private"* variables. - cit *W3School*