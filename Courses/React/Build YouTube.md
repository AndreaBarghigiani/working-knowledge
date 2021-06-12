The application we're going to build is a full stack application with code for server and client.

The server code is kept inside the `server/` folder and the definition of our ExpressJS server is kept in `server/src/start.js`:
```js
import "express-async-errors";
import path from "path";
import cors from "cors";
import logger from "loglevel";
import morgan from "morgan";
import dotenv from "dotenv";
import cookieParser from "cookie-parser";
```
Those are some of the middleware we're loading to make easier work with the server code, we also use Babel to make the Node.js code that we will write more similar to the one that we will put for the frontend.

Inside the app we will build also some API thanks to the `getRoutes()` fn that is defined in `server/src/routes/index.js`. 

If you want to dig deeper into how the project is set up check the comments in the source code.
## Creating login routes
In order to login our user must send us an `username` and an `email`, those data will be handled by Express when we define a post request for the endpoint `/google-login`.

```js
function getAuthRoutes() {
  const router = express.Router();
  
  router.post('/google-login', googleLogin);
  
  return router;
}

async function googleLogin(req, res) {
	const { username, email } = req.body;
	// More code that handles the request 
}
```
Once we get those from the user request we need to check in our database if user with that username or email actually exists, we can do that with the Prisma client.
```js
import express from "express";
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

function getAuthRoutes() {
  const router = express.Router();

  router.post("/google-login", googleLogin);
  return router;
}

async function googleLogin(req, res) {
  const { username, email } = req.body;

  let user = await prisma.user.findUnique({
	  where: {
		  email
	  }
  })
  
  // Other code...
```
As you can see in order to work with the Prisma client we need to import it and instantiate it in the `prisma` variable.

Now we can query our database based on the schema that we already provided for our database. First we check with the method `findUnique` if the user already exists in our database, if we do not find any user we will create it.
```js
if (!user) {
	user = await prisma.user.create({
		data: {
			username,
			email,
		},
	});
}
```
We will update our `user` variable with the result of the creation of the user, **keep in mind** that all the prisma client queries are `async` and returns a Promise. In this code we handle this with the `async/await` syntax but you could also use a `then()`.

Now that we check if the user is already present or not we need to create a JWT.

First we will import the library `jsonwebtoken` and then we can create our key as following:
```js
import jwt from "jsonwebtoken";

async function googleLogin(req, res) {
  // Other code...
  
  const tokenPayload = { id: user.id }
  const token = jwt.sign(tokenPayload, process.env.JWT_SECRET, {
    expiresIn: process.env.JWT_EXPIRE,
  });

  res.cookie("token", token, { httpOnly: true });
  res.status(200).send(token);
}
```
Once created the token and stored it in the `token` variable, if you want to dig deeper in JWT token have a look at the docs, and then we use this variable as a response from our server and also to store a cookie that have the same name as the variable.

### Current user on `/me`
We can add this new request with a simple `router.get` call but in this case we want to add a middleware that will protect our route by running some logic before send the request.

To do so we already have a fn declared in `server/src/middleware/authorization.js` called `protect`.
```js
export async function protect(req, res, next) {
  // The code to run before...
}
```
In future we will use cookies but for the time being we will pass the auth token via headers and the first thing we need to do into our `protect` middleware is to check if we have it from the request:
```js
export async function protect(req, res, next) {
  if (!req.headers.authorization) {
    return next({
      message: "You need to be logged in.",
      statusCode: 401,
    });
  }
}
```
Now that we notice it we need to validate the token that is passed:
```js
import jwt from "jsonwebtoken";
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

export async function protect(req, res, next) {
  // Other code...

  try {
    const token = req.headers.authorization;
    const decoded = jwt.verify(token, process.env.JWT_SECRET);

    const user = await prisma.user.findUnique({
      where: {
        id: decoded.id,
      },
      include: {
        videos: true,
      },
    });

    req.user = user;
    next();
  } catch (error) {
    next({
      message: "You need to be logged in.",
      statusCode: 401,
    });
  }
}
```
If you check `prisma.user.findUnique` this time you'll not only see that we are looking for the user with the current id **but** we are also requiring to include the relationship with the Video table.

Once we have the `user` we can pass it to the next controller adding it to the current `req` and passing it along with `next()`.

## Create the Video API Routes
All our endpoint will be `api/v1/videos`.
### Get recommended videos
The first endpoint we will work with is the one that will get the recommended videos, we do not want to limit this only for authenticated users so we will leave this open. With the help of Prisma we can prepare the query like so:
```js
await prisma.video.findMany({
	include: {
		user: true
	}
})
```
This time we query the `video` table just to include all the video related to the current user.

We also want to order the videos to be ordered descending and we want to handle also the case when no videos are found, so the complete `getRecommendedVideos`, that we pass to the root of the endpoint, is:
```js
async function getRecommendedVideos(req, res) {
  let videos = await prisma.video.findMany({
    include: {
      user: true,
    },
    orderBy: {
      createdAt: "desc",
    },
  });

  if (!videos.length) {
    return res.status(200).json({ videos });
  }

  res.status(200).json({ videos });
}
```
### 10. Create get video views utility function
Since the view count is stored in a separate table we need to query that table for each video we retrieve, this is the reason that we exit early from the `getRecommendedVideos` if the `length` of our response is 0.

To count something in Prisma you just have to use the `count` method passing to it some configuration, in our case this is the query to get the views count for each video:
```js
await prisma.view.count({
  where: {
	videoId: {
	  equals: video.id,
	},
  },
});
```
This will get us what we're looking for **but** we miss. an important information, we miss the `video.id`. To get this we need to loop through the array of videos and since we will be using this logic many times inside our application is better to create an utility function:
```js
async function getVideoViews(videos) {
  for (const video of videos) {
    const views = await prisma.view.count({
      where: {
        videoId: {
          equals: video.id,
        },
      },
    });

    video.views = views;
  }

  return videos;
}
```
The `getVideoViews` is transforming our data adding a `view` property to each object. We can simply call it from `getRecommendedVideos` and we will have the view count for each video:
```js
async function getRecommendedVideos(req, res) {
  let videos = // Prisma query...

  if (!videos.length) {
    return res.status(200).json({ videos });
  }
	
  // Now I can count views because videos is not empty
  videos = getVideoViews(videos);

  res.status(200).json({ videos });
}
```
### 11. Get trending videos by view count
In this lesson we want to sort our videos by the number of views with the `getTrendingVideos`, this function is almost identical to the `getRecommendedVideos` except that we `sort` the data before to return.

So we just need to add the following:
```js
videos.sort((a, b) => b.views - a.views);
```
### 12. Search videos by title or description 