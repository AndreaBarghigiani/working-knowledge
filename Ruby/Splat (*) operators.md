# Simple * splat
Maybe the first common use of the `*` spat operator is the ability to create a method that accepts an undefined number of arguments.
```ruby
def splat_arguments(*args)
	args
end
```
But it can be useful to deconstruct arrays too:
```ruby
first, *remainder = [1, 2, 3, 4, 5]
# Produces 
# first = 1, remainder = [2, 3, 4, 5]

*prefix, last = [1, 2, 3, 4, 5]
# Produces 
# prefix = [1, 2, 3, 4], last = 5

first, *, last =  [1, 2, 3, 4, 5]
# Produces 
# first = 1, last = 5
```
As for the last example, if we do not give a name to the splat operator Ruby just does not create a variable to store the values in it.

Other fun fact of the splat operator is that you can create arrays too:
```ruby
[ *[1,2], *[3,4] ] = [ 1, 2, 3, 4 ]
*"hello world" = [ "hello world" ]
*{key: 'value'} = [ [ :key, 'value' ] ]
```
## Deep dive
* https://alexcastano.com/everything-about-ruby-splats/
* https://www.honeybadger.io/blog/ruby-splat-array-manipulation-destructuring/