## Everything is an object

#### No primitive value
```ruby
1.class #> Fixnum

(1.5).class #> Float

true.class #> TrueClass
```
This is the reason we can write something like this:
```ruby
3.times do
	# something
end

class Integer
	def times(&block)
		# for "self" times run a block
	end
end
```
## Classes
#### Open Classes
You can extend and override methods in a class by simply open a new definition in one of the project file:
```ruby
class String
	def foo
		"bar"
	end
end

"Some string".foo #> "bar"

class Array
	def reverse
		raise "We don't do that here."
	end
end

["one", "two"].reverse #> RuntimeError: We don't do that here.
```
#### Duck Typed
As far as an object responds to method calls it is not important the class type.
#### Definition are just Runnable Code
You can insert standard Ruby code in a class definition and it will get executed even if there is no instantiated object:
```ruby
$SPECIAL_CASE = false

class MyClass
	puts "Running code in a class"

	2.times { puts "What?"}

	def special_method
		#something special here
	end if $SPECIAL_CASE
end
```
When this file gets loaded interesting things happen:
```ruby
> ruby my_class.rb
Running code in a class
What?
What?
	
> MyClass.new.special_method
#> NoMethodError: undefined method 'special_method'
```
The call to the `special_method` raise an error because since `$SPECIAL_CASE` is `false` the entire method definition is not considered.

Basically the class definition is changing depending on some logic, really important aspect to remember to **metaprogram in Ruby**. 