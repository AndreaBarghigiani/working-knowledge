# Key Feature & Benefits
- **server side rendering** - generates the HTML in the server and not on the client side
- **file based routing** - files and folder defines the website paths
- **fullstack capabilities** - you can write backend code to define API

# Routing
In Next.js we have a file-based routing, this means that the routes of the project are defined by the structure we set in the `pages/` folder.

The main `index.js` file acts as the root of the folders it lives in. The `index.js` that lives inside `pages/` will be the homepage of the project but if we have the same filename in `pages/products/` it'll act as the index page of that subfolder.

For example, if the domain is `mydomain.com` the `pagex/index.js` will be the index of the domain while `pages/products/index.js` it'll be the index for `mydomain.com/products/`.

We can define all the subpages with the filename, `pages/about.js` will create the `mydomain/about` route while `pages/products/shoes.js` will create `mydomain.com/products/shoes`.

Next.js has a special way to generate routes dynamically and it's done using the square brakets, `pages/products/[id].js` will generate dynamic routes based on the `id` of the product: `mydomain/products/1`.

## Dynamic Routes
We've seen how powerful a dynamic route can be, but we also want to use the data that has been passed and for this Next.js gives us an Hook from the `next/router` that's called `useRouter`.

With it we can get many information about the route we're currently checking, generally the most important information are inside the `query` object:
```js
import { useRouter } from 'next/router'

export default function Page(){
	const route = useRouter()
	
	console.log(route)
}
```

Inside it we will find **all the parameters we add as dynamic** into our route, so for example if we created a file `products/[productid].js` the log will give us this object `{productid: 'whatever'}` where *whatever* is defined in the route as `mydomain.com/products/whatever`.

But we can even **nest dynamic values**, for example we can have `products/[productcat]/[productid]` and if we go to `mydomain.com/products/shoes/fancy` our query object will have the following values: `{productcat: 'shoes', productis: 'fancy'}`.

Even more we **can have catch-all routes** that will let us define **any kind of URL** right from the name `[...slug]`. To be clear, *slug* can be named anything you want, the important part is the [[spread operator]] that we put in front of the name.klkl 