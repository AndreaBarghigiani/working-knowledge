Sometime is useful to know which action has rendered a view, you know maybe you need to set some specific code just for that action, and you can do so by checking the `action_name` value.

To make things real let's say that you have a form that is hidden by default and the user needs to check a checkbox to see it. Beside the JS (em... Stimulus) stuff you need to put in place for this, one thing that you would not force you users to do is to check it again in case the `create` action is not able to `save` your resource.

The starting point is this: 
```ruby 
# _controller.rb

def new
	# This renders new.html.erb view file
end

def create
	if resource.save
		redirect_to "wherever you want"
	else
		# Got an error, user must. fix it
		render :new
	end
end

# view/resources/new.html.erb

<label class="form-checkbox">
	# This checkbox removes --is-hidden class from the form and makes it visible
	<input
		type="checkbox"
		class="js--show-from"
	>
	Jag har läst och förstått
</label>
			
<%= simple_form_for(@resource html: { class: '--is-hidden' ) do |f| %>
```
Problem is that when `create` renders the `new` view the checkbox is still unchecked and `form` has `--is-hidden` class applied to it.

But we want to change that if the view is called from the `create` action. 

To do so we just need to check the `action_name` in our view and we can change things:
```ruby
<label class="form-checkbox">
	<input
		type="checkbox"
		<%= 'checked' if action_name == 'create' %>
	>
	Jag har läst och förstått
</label>
<%= simple_form_for(@resource,html: { class: action_name == 'create' ? '' : '--is-hidden' }) do |f| %>
```
Here you go, now the view is aware of the action that has rendered it and is able to behave accordingly.