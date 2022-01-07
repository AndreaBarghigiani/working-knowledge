A `Proc` object berhaves somewhat like a method: you define some behavior and then you can use that repeatedly. Let's look at a simple example:
```ruby
square = Proc.new { |x| x**2 }
square.call(4) # 16
square.call(5) # 25
```
By [definition](https://ruby-doc.org/core-2.6/Proc.html) a `Proc` is:
-  an encapsulation of a block of code
-   which can be stored in a local variable
-   or passed to a method or another Proc
-   and can be called

Getting into each point:
### A `Proc` object is an encapsulaation of a block of code
The code in a `Proc` object is packaged up and isolated from the code outside it.
### A `Proc` object can be stored in a local variable
Like in our first example, the block that we pass to the `Proc` is stored in a variable called `square`
### A `Proc` object can be passed to a method or another `Proc`
Here's the example on how two different `Proc` objects can be passed to a method:
```ruby
def perform_operation_on(number, operation)
  operation.call(number)
end

square = Proc.new { |x| x**2 }
double = Proc.new { |x| x * 2 }

puts perform_operation_on(5, square)
puts perform_operation_on(5, double)
```
Instead of passing *data* as a method argument, we're passing *behavior* or to say like before *we pass an encapsulation of a block of code*. It's then up to the method to call (hint the `call`) the block or not.

But statements says that we can also pass a `Proc` to a `Proc`, here's the code above refactored to reflect this statement:
```ruby
perform_operation_on = Proc.new do |number, operation|
  operation.call(number)
end

square = Proc.new { |x| x**2 }
double = Proc.new { |x| x * 2 }

puts perform_operation_on.call(5, square)
puts perform_operation_on.call(5, double)
```
Only difference here is since `perform_operation_on` is defined as a `Proc` we have to `call` it to execute the code.
### A `Proc` object can be called
As we saw in previous examples a `Proc` can be called with the `#call` method, this executes the code withing the block.
# Resources
- https://www.codewithjason.com/ruby-procs/