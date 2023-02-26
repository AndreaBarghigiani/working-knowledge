**Install CLI**
```
npm i -g wrangler
```

**Preview**
```sh
wrangler dev
```

**Deploy**
```sh
wrangler publish
```

We must always respond with a `Response` from an `async` function.
The `Response` can also be an HTML page, but in order to do so we have to create a `template.js` file that returns a **string**.

Once done that you can `import` the resource as you're used to do in any React.js application with ESModule. Just make sure that your `Response` has an `header` that specifies the `content-type` as `text/html`.

## `request.cf`
Inside the `request` attribute that will have our function that handles the `Response`, like information on where the response comes from and other useful information 