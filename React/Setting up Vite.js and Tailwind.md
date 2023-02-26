Here I describe my latest (updated 13/2/2023) way of setting up a React.js project with Vite.js, TailwindCSS, Axios, React Query, and Supabase.

First and foremost, let's install React.js:
```sh
npm create vite@latest
```
Follow the instructions to setup the project, then `cd` into the new folder and run `npm i`

## Install TailwindCSS
This is also a simple step. Inside the project folder, type:
```sh
npm install -D tailwindcss postcss autoprefixer
```
Once the installation has ended, let's setup TailwindCSS:
```sh
npx tailwindcss init -p
```
Now open your editor, I use VS Code, and the command is:
```sh
code .
```
Now open the `tailwind.config.cjs` file and add the `path` to watch for the TailwindCSS classes:
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./index.html", "./src/**/*.{jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```
Vite.js forces you to use the `.jsx` or `.tsx` extension. Just keep the one you'll use.

It's time to change the `.css` file. If you're like me drop every CSS file except the `index.css` one where you'll replace its content with:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
Now you can replace the content of your `<App />` component and test if everything works out.
```js
function App() {
  return <h1 className="text-bold underline text-4xl">Hello Andrea</h1>;
}

export default App;
```
Another thing that I generally do is to setup some classes for the `<body>` element in `index.html`.

> **Suggestion:** Don't forget to install Prettier and the `prettier-plugin-tailwindcss` to reorder the classes upon save.
> ```
> npm install -D prettier prettier-plugin-tailwindcss
> ```

> **Optional:** You can install the [Twin.macro](https://github.com/ben-rogerson/twin.macro) package to leverage the power of styled-components with TailwindCSS

## Axios and React Query
It's time to use the best tools for the job when talking about REST API calls. Type the following in the terminal:
```sh
npm i axios react-query
```
if you're looking to organize your code better, I suggest creating an `/api` folder inside your `/src`. This way, all your API calls live inside that, making it easier to pick and choose the best one for the job.

## Supabase
Nowadays, having an application without a database is useless, and remember that Supabase **is much more** than a simple database. One of the features that I commonly use in my project is also the authentication 🎉
```sh
npm i @supabase/supabase-js
```
After you've done this create a `scr/api/supabase.js` file and start to create the API calls to make use of this powerful tool.