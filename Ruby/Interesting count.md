When used with an enumerable the `count` method simply return the number of items but if you pass a [[block]] to it it will count only the values that yields a true value.

```ruby
numbers = [1, 2, 3, 4, 5, 6]

numbers.count { |num| num > 3 } # Counts 3
```
