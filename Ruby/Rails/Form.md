# From Forms into `params`
Since each field in a form **must have** a `name` attribute, this will be the key that Rails uses when creating the `params`. For example:
```html
<input type="text" name="description" />
```
Will create a `params[:description]` that you can refer to into the controller that will handle the data.

Having a single name for each field is not always useful, most of the time you want to send a set of fields in the form of an Hash. This simplify a lot your code but at the same time bring some more code because Rails will try to protect our application from malicious users.

In order to do so, in our form we can simply set the `name` attribute with a specific value:
```html
<input type="text" name="user[fist_name]" />
<input type="text" name="user[last_name]" />
<input type="text" name="user[email]" />
```

Then we will send this form we will see an Hash `user` inside the `params` that holds all the data. Doing so requires a bit of code in our controllers to handle (and secure) the data we receive and we can do this with the `require` and `permit` methods.

In the controller we use `require` to specify the name of the object and `permit` to list all the `name` of the fields we will accept.

# Nested Form
- go to the model where we want to inject the atttribute and add `accepts_nested_attributes_for :symbol_of_model`, this is defined by the `has_many` association we made above
- go to the controller and add the field that we want to `permit` passing in them an Array that is stored in the key that has the same key of the model followed by `_attributes`. So if `:symbol_of_model` is the model associated we can write something like it: `params.require(:model).permit(:title, :description, symbol_of_model_attributes: [:id, :name, :description])`
#### Checking data sent by a form
Generally speaking you can debug what data your form is sending right from the logs that your Rails app sends once you started a server `rails s`. This is a common log of a form sent via a `POST` that has a `text` input named `email`:

```bash
Started POST "/user" for 127.0.0.1 at 2013-11-21 19:10:47 -0800
Processing by UsersController#create as HTML
Parameters: {
	"utf8"=>"✓", 
	"authenticity_token"=>"jJa87aK1OpXfjojryBk2Db6thv0K3bSZeYTuW8hF4Ns=", 
	"email"=>"foo@bar.com", 
	"commit"=>"Submit Form"
}
```
As you can see you get many informations.

First line tells us the HTTP method used and the route the form went to.

In the second one we see the controller and the action that process the data sent via the form.

And in the third like, that I expanded to improve readibility, we see all the data that will populate the `params` hash that the controller will use.

#### Gotchas
- in Rails you **must use the helper methods** to generate the form and all its fields because as a standard security it protects us from the [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) and generates an `authenticity token` in each form as an hidden input.
- `name` is a required field for your forms since let Rails know how to call the data you insert in the input so it can add it to the `params`
## Resources
- [Form basics on OdinProject](https://www.theodinproject.com/paths/full-stack-ruby-on-rails/courses/ruby-on-rails/lessons/form-basics)