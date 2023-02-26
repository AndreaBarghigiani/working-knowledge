In some cases you need to add a separator between two string, in normal HTML this could be solved in the following:
```html
<span class="event-card__start-date"><%= next_date %></span>
-
<span class="event-card__end-date"><%= last_date %></span>
```
But when woking with Ruby and Rails there is a better way to do this, especially if the data you need comes from a [[Model]]. In this example we're working with ViewComponent and we created a method in the `.rb` file of it:
```ruby
def dates
	[
	  (tag.span(next_date, class: 'event-card__start-date') if next_date),
	  (tag.span(last_date, class: 'event-card__end-date') if last_date && next_date != last_date)
	].reject(&:blank?).join(' - ')
end
```
This created dinamically the tags and rejects all the emtpy (`blank?`) strings, if the array needs to `join` the strings then we use the `-` as a separator.