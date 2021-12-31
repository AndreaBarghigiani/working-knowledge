The `partition` method let us divide an array by the matching condition. If you store the result in a single variable you'll get an array that contains two array, one with the matching elements and the other without.  But the killer feature is that you can **deconstruct** the result into two separate variables.
```ruby
nums = [1, 2, 3, 4, 5, 6]
small_nums, big_nums = nums.partition { |n| n < 3 }
```