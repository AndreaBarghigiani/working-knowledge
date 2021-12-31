To work with conditionals is always best to convert the value we have in a variable into a boolean. 

If you expect a variable to **not be `nil`** you can leverage the `nil?` method. Naturally `nil?` returns `true` if the value is `nil` but if you're looking to have the answer to *is it **not** `nil`?* then you can simply negate it:
```ruby
!var.nil?
```