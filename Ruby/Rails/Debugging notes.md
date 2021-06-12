There are many way to debug a Rails app and mostly it all depends on where we want to see the debugging information.

In this note I am approaching the fastest way to debug something, if you want to dig deeper you can [have a look at the Rails guide](https://guides.rubyonrails.org/v2.3/debugging_rails_applications.html) that it is really extensive.

# Debug in the view
## `debug`
The first approach is to use the `debug` helper that will work really similarly to the `var_dump` that comes in PHP.

As for `var_dump` also `debug` **must be used in a view** since it will create all the `pre` and `code` tags that will help you read through the code.

All we need to do is to pass to `debug` something to analyze:
```erb
<%= debug(@var) %>

<%= debug(params) %>
```
Now we pass the instance variable `@var` so we can see all the things that are contained in it. As you can see we also check the `params` hash, we [[The params hash||already had a look at it]], and with it we can easily discover all the information such as the controller, action and others that we can get from `params`.

A different approaches that I will not look to use in future, just adding for reference, are:
* `simple_format` and `to_yaml`- this shows similar information as `debug` but you will have to wrap in a `pre`
* `inspect` - let's us print the structure of any kind of data as a simple string on the screen, works best to debug array and hash.

## `web-console` Gem
This is a kind of standard gem that generally is used to have an `irb` console right in the webpage so you can pass the variables used in the view to check where things get off.

Generally this element will display in an error page but if for some reason you need to see it in a specific view you just have to place the following snippet anywhere in the page:
```erb
<%= console %>
```
# Debug for controller and model
## `byebug` Gem
This is a complete solution to test how our methods are going to play out. We can use it from console with `byebug myscript.rb` or insert it in the method we want to debug:
```ruby
class BaseController < ApplicationController
	def index
		byebug
		# Do other stuff
	end
end
```
Once the `BaseController#index` gets called **the entire application** pauses and you're able to let it run step by step with your console, the one where you lunched `rails server` 😉

This Gem gives you many option, too many to describe here, so if you are interested to dive deep you can type `help` while the app is on halt or check the [README file in the repo](https://github.com/deivid-rodriguez/byebug#byebugs-commands).

If you're familiar with debugging JavaScript application with the `debugger` keyword (or by placing breakpoints right in your browser) then you can bedder understand the behavior.

In closing, as for the opening, debugging in Ruby can take many forms and shapes. Here I listed the ones that I feel are more user friendly and more used but have a look in the Rails site, there are rubies in there 😉