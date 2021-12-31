You can automatically create a select with the associated database information as long as the model you're using has that relationship defined.
In [this example app]() you see that I have an `Invoice` model that has those relations:
```ruby
class Invoice < ApplicationRecord
  belongs_to :user
  belongs_to :company
  belongs_to :client
end
```
The `User` is automatically set to the current logged user but we also have `Company` and `Client` so in order to display a select to offer an UI to the user all you need to do with `simple_form` is:
```ruby
<%= simple_form_for @invoice do |f| %>
	<%= f.association :client %>
<% end %>
```
The code above will display a select with a label already populated.

If you want to hi