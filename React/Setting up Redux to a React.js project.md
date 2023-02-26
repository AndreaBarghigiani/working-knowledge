Now that we have a React.js project, it's time to set it up with RTK:
```sh
npm i @reduxjs/toolkit react-redux
```
Now that we have the right tools, it's time to create the store for the application. Create a `scr/store/index.js` file and create it like so:
```js
import { configureStore } from "@reduxjs/toolkit";

const store = configureStore({
  reducer: {},
});

export { store }
```
Now that the store is configured, we need to use it inside our application with the help of `react-redux`. Open the `main.jsx` file and transform it like so:
```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";

// Redux
import { store } from "./store";
import { Provider } from "react-redux";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```
Thanks to the `react-redux` library we are able to leverage the Context API of React.js and connect the `store` to it.

Now you have your store connected to the entire application but in order to move on you have to create a **slice**.

## What's a Slice?
I `slice` is just a part of your store that has a reason to exist, in order to better handle all the actions involved in the update and modifications of your `store` you generally create a new slice.