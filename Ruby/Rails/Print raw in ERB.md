Sometimes you have a string that you would like to print a string as is, without having Rails validating it. Generally you can do a `raw` call:
```ruby
<%= raw '<h1>Helloo</h1>' %>
```

But there is a quicker approach, all you have to do is to use a second `=` and you're ready to rock:
```ruby
<%== raw '<h1>Helloo</h1>' %>
```

This is also useful, while working with ViewComponent I had the need to wrap the components, checking the `_iteration` param I was able to add some onliner logic to create the container.

```ruby
<%== '<ol class="funding-checklist__answers">' if @iteration.first? %>
 # Custom code
<%== '</ol>' if @iteration.last? %>
```