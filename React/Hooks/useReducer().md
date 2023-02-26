**[Full Docs](https://beta.reactjs.org/reference/react/useReducer)**

Hook lets us handle the state of our application in a Redux-like way. Basically, we store the state and we edit via some actions that we dispatch in it. 

Will be back with an improved explanation, but if you know Redux you're pretty close 😉

```js
const [state, dispatch] = React.useReducer(reducer, initialArg)
```

The `useReducer` hook accepts three arguments:
- `reducer`: this is the fn that will be in charge of updating the state. It takes the state and the actions and will return the next state.
- `initialArg`: as a normal [[Useful array methods#^224568|reduce]] fn, here we can define the initial value.
- `init`: *optional* and, like [[useState()#^dee6a4|useState()]], we can improve performances of the following renders defining an initializer function.

The `useReducer` **returns** an array with two values:
- **current state** calculated
- **dispatch function** that allow us to update the state