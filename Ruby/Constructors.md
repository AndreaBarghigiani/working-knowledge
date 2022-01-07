Each time we instantiate an object from a class we call the `new` method on it, but as end user we know that `initialize` is the method that get's called as constructor of the class.
```ruby
class Point
	def initialize(x, y)
		@x = x
		@y = y
	end
end
	
#> Point.new(3,5)
```
The cool thing is that we can **customize the `new` method** for our objects, because behind the scenes when we call `new` we `allocate` space in memory.
```ruby
class Point
	def self.my_new(*args, &block)
		instance = allocate
		instance.my_initialize(*args, &block)
		instance
	end
	
	def my_initialize(x,y)
		@x = x
		@y = y
	end
end
```
Here we've created `my_new` class method that:
1. allocate a new empty object of the class
2. run specialized initialization
3. return the instance

In this case we accept any number of arguments with the splat operator `*args` and we also pass any block provided to the `my_initialize` with `&block`. Consider also that `my_initialize` is a kind of callback concern only instance instantiation.

We can also leverage this tecnique to make sure to create a single instantiation of the class, this is the `Singleton` pattern, by making the `new` method private.
```ruby
class SnowFlake
	class << self
		private :new
	end

	def self.instance
		@instance ||= new
	end
end
```