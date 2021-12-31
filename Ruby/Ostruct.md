`OpenStruct` is part of the standard library and allow us to create objects from `Hash` so instead of having to use the `Hash` keys (es. `hash[:key]`) `OpenStruct` allows us to use methods.

If I am not wrong this is what all the `attr_*` declarations do for us inside a class but if you want to convert an `Hash` into an object that has those methods you have to explicitly `require` and set this:
```ruby
require 'ostruct'

attributes = {name: "Andrea Barghigiani", age: 35, location: "Italy"}
person = OpenStruct.new(attributes)

# Now you have access to setters and getters

person.name # Instead of attributes[:name]
#=> Andrea Barghigiani

# Update/Set the age
person.age = 38

person.age 
#=> 38
```

When using the [[block syntax]] to call a single method you can use a shortcut replacing the entire block with `&:` followed by the method name:
```ruby
# Long form
peolple.sum { |person| person.age }

# Shortcut
people.sum(&:age)
```