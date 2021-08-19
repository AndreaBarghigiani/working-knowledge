I was curious about this, generally speaking it was advised to me to prefer the use of `collection` instead of looping over a list of items. I tryed to take it for granted but the **why** question popped up from time to time.

The base concept is to improve the **memory usage** because if we loop over an item and then call the template that needs to render the information in it we will **initialize the template each time**, instead if we go with the `collection` route we will initialize the template just once and it will be reused to render all the object in the collection.

So each time you feel the need to write something like this:
```ruby
<% @items.each do |item| %>
	<%= render 'item', item: item %>
<% end %>
```

Try to write it like so:
```ruby
<%= render partial: 'item', collection: @items, as: :item %>
```

Not only the code in your view will look cleaner but you will speed up your application, depending on the number of items you could save a lot of time.

> On a simple test with a thousands of `ActiveRecord` this change has improved the performance **more than 90%** 😮

### This is important also in ViewComponents
The concept explained here is important for the ViewComponents as well so remember to keep in mind.