In RoR the term *resource* is really common. One of the first places where you find this term is in your `routes.rb` but basically is used all over the place even if sometime does not keep the same name.

Basically a *resource* is any kind of object that can be accessed via a URI and is expected to take in CRUD operations.

In simple terms, at least in the RoR world, a resource is generally a database table that it can be represented by a model and accessed by a controller.

For example, the **Post** resource in RoR is linked with a `posts` table in the database, it will have a controller `posts_controller` and generally will create routes like `/posts`.

Once we set up our resource like so we can create new routes for it by simply adding `resources posts` in our `routes.rb`, just like explained in [[5. Read from the FrontEnd (the show action)#^ae47a5|this section]].