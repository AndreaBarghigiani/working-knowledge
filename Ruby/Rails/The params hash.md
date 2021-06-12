Is a collection of data that comes to our app with the request and it can be come from various places:
* within a `form` that has been sent
* from the URL that contains some data in the path or GET params

Let's say we're in the following situation, we are developing a blog and we create a route, a controller and a view (the normal stuff):

*`config/router.rb`*
```ruby
Rails.application.routes.draw do
	get '/blog', to: 'blog#index'
end
```
*`app/controllers/blog_controller.rb`*
```ruby
class BlogController < ApplicationController
	def index
	end
end
```
*`app/views/blog/index.html.erb`*
```ruby
<h1>Blog</h1>
```

### `GET` request 
A `GET` request parameter are all the key/value pair you find after the `?` in your URL. With Rails is enough to have this data in the URL to see the difference inside our server log:
```bash
Started GET "/blog?name=andrea" ...
Processing by BlogController#index as HTML
Parameters: {"name" => "andrea"}
...
```
So now we can jump back to our `BlogController` and make use of the data with the `params` hash:
```ruby
class BlogController < ApplicationController
	def index
		@name = params[:name]
	end
end
```
Now we store the value of the `name` parameter that we pass via GET and store it in the instance variable `@name` so we are free to call it from our view as well:
```ruby
<h1>Blog</h1>

<%= @test %>
```
### The route parameters
Let's say that we want to add a new route, the `/blog/:id` one, we are telling to Rails that we want the part **after the `:`** to be stored into the `param` hash. Here's an example on how we can handle this situation.
*`config/router.rb`*
```ruby
Rails.application.routes.draw do
	get '/blog', to: 'blog#index'
	get '/blog/:id', to: 'blog#show'
end
```
*`app/controllers/blog_controller.rb`*
```ruby
class BlogController < ApplicationController
	def index
		@name = params[:name]
	end
	
	def show
		# I can use the id with params[:id]
		@id = params[:id]
		@name = params[:name]
	end
end
```
> We can mix GET and route parameters, it is just a matter on how you handle the data.

### Form parameters
Let's fake the creation of a new post for our blog, before all we create the new route:
*`config/router.rb`*
```ruby
Rails.application.routes.draw do
	get '/blog', to: 'blog#index'
	get '/blog/:id', to: 'blog#show'
	post '/blog', to: 'blog#create'
end
```
Once the new route is in place it's time to add a form in our index view:
*`app/views/blog/index.html.erb`*
```ruby
<h1>Blog</h1>
<%= form_with blog_path do %>
	<div>
		<%= text_field_tag :title %>
	</div>
	
	<div>
		<%= text_area_tag :body %>
	</div>
	<%= submit_tag "Create Post" %>
<% end %>
```
*`app/controllers/blog_controller.rb`*
```ruby
class BlogController < ApplicationController
	# Previous code...
	def create # We just make a redirect
		redirect_to action: :index
	end
end
```
Now if you send the form we have more data that we provided that are used for internals or advanced stuff, the important thing is that `:title` and `:body` are there and we can grab as any other `params` we see here.
