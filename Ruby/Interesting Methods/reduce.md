The `reduce` method looks really similar to the JavaScript one, iterates over all the elements in an Array and return a single value based on the operation we do on them.

The easiest form of this method is the sum of many numbers but in Ruby we have the ability to make our code even shorter.
```ruby
nums = [1, 2, 3, 4]

sum = nums.reduce(0) { |sum, n| sum + n }

p sum # 10
```

In the above example we used the long form of `reduce` where you set the initial value as method parameter and you use two block parameters for the accumulation variable and the one that represent the current element.

But `reduce` can be written in a shorter way:
```ruby
sum = nums.reduce(:+)
```

First and foremost we do not assign an initial value because if ommited the `reduce` method will automatically use the first value in the array as initial but we also do come more magic here.

We have dropped the block all together and use the operator that we want to execute for each item as a method parameter.

> **Note:** In this case we used an operator but it'll be valid to use even any kind of method that you can use with the object you are iterating in.

As you can see the `reduce` method have great potential and we can use it to simplify your code.