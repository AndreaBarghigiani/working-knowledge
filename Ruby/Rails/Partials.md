Partials are the standard Rails way to separate code into reusable components, lately we're using ViewCompoents to do so but it's good to have some retrospect on this.

## Create a partial
You can create a partial **anywhere inside the `view/` folder**, all you have to do is start the name of the template with an *underscore* `_my-partial.html.erb`.

## Load a partial
You can load your partial as part of the view, means that you already load a view file with an action, using just the `render` method: `<%= render "my-partial" %>`.

As you can notice the underscore and extension are left behind and if you're using different folders you have to insert the path.

## Pass variable
You can also pass local variables to it, the syntax changes just a little bit because now you pass multiple parameters:
`<%= render partial: "my-partial", locals: { post: @post }`

## Object partials
One of the best feature of partials, if you get used to, is that you can pass to it an object or collection of an ActiveRecord model. All that you need to make sure for the object is that it responds to `to_partial_path` and you can start loading your partials like so: `<%= render @post %>`

Since `@post` is an object of ActiveRecord by rendering it we are telling Rails to load `views/posts/_post.html.erb`

Rendering partials like so will automatically give us a local variable named `post`

We can also render collections, in the case we pass the full `@posts` Rails will be able to consecutively render each item in it with the `view/posts/_post.html.erb` partial.

## Global partial
If you cannot refer to an object that responds to `to_partial_path` and you don't want to remember the full path of your partial then you can always define a global one.

First and foremost you need to move the partial into `views/application` and then you can call it anywhere by just typing it's name: `<%= render 'my-partial' %>`
