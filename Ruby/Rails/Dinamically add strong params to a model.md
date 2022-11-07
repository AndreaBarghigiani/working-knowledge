Adding strong params to a Model it can be a pain, if you change the model you also have to go and change the attributes.

While this can be good in a way since allows you to 'keep track' of the data you store while you're just trying some stuff out it can just be a pain. 

Luckly us we have the nice `attribute_names` method that is able to **return an array** of all the attributes of that specific model. So if you have an User Model all you have to do to get all its attributes is:
```ruby
User.attribute_names
```

This returns an array but the `permit` method used works better with symbols and our array is full of strings...

To fix this we just have to convert the values in the array like so:
```ruby
User.attribute_names.map(&:to_sym)
```

dd