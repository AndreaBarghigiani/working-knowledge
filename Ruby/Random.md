> Notes based on [freeCodeCamp.org YouTube video](https://youtu.be/t_ispmWmdjY). The course is focused on syntax without the web part.

Scripting language that executes the instructions line by line.

`puts` is a way to `print` but different from the latter because it adds a line brake automatically.

### Variables 
When we create a variable in Ruby we need:
* *name* - separate multiple word with `_`
* `=` - used to assign
* *value* - the actual value stored in the var

```ruby
// Variable declaration
person_name = "Andrea"
person_age = "35"
```

`puts` works like a normal fn in JS, if we concatenate two or more strings requires parenthesis:
```ruby
puts( "My name is " + person_name )
puts( "and I am " + person_age + " years old")
```

### Data types
The kind of data we can work with in Ruby.

#### String
As usual, just text.
```ruby
text = "Andrea"
```

#### Integer
```ruby
num = 15
```

#### float
```ruby
pi = 3.14
```

#### boolean
```ruby
bool = true // (but it could be false)
```

#### nil
Basically the `null` in JS. Parenthesis are optional but considered good practice.

### Working with strings
A collection of the most common ways to work with them 😉

As already saw `puts`, like `print`, allow us to print the string on the string.

As for other languages I can use escape characters that are pretty similar in the JS world. The general escape char is `\`

You can concatenate strings with interpolation as well like we do in template string in JS:
```ruby
name = "Andrea"
puts "Hello my name is #{name}"
```

#### `upcase`
The correspondent of `toUpperCase` JS fn.

#### `downcase`
The correspondent of `toLowerCase` JS fn.

#### `strip`
The correspondent of `trim` JS fn.

#### `length`
Same as in JS.

#### `include`
This works like `match` in JS but uses `?` instead of parenthesis:
```ruby
name = "Andrea Barghigiani"
name.include? "Andrea" // Returns true
name.include? "Barghigi" // Returns false
```

#### `[]`
As in JS if I use `[]` and used a number within them it'll return the char at specific index, strings have `0` based index.

But differently from JS I can also pass a range to have a range within the string length, note that last index is ignored.
```ruby
name = "Andrea Barghigiani"
puts name[0,3] // Prints "And" (end index is ignored)
```

#### `index`
Returns the `index` of the starting index of the provided string, like `indexOf` in JS.

### Numbers
All the standard operations are in place and works like in JS.  If you use integer an integer will be returned no matter what, if I run `7 / 3` I'll get an integer, if I want a float one of the numbers must be float.

Unlike JS though when I want to use a number in a string I **always** have to convert it to a string with `to_s`:
```ruby
num = 15
puts ("my number is" + num.to_s)
```

#### `abs`
Like in JS returns the absolute value,

#### `round`
Like in JS removes the floating part of the number by rounding it to the appropriate value:
```ruby
num = 14.01
num.round() # Returns 14
num = 14.625
num.round # Returns 15
```

#### `ceil` and `floor`
Work exactly the same as JS.

#### `Math` class
The class to run mathematical operations, just like in JS.

### Getting user input
I can show a prompt and assign the typed value right inside a variable with `gets`:
```ruby
name = gets
```
`gets` takes the *Enter* character and uses it, if you do not care about it you can use `gets.chomp()`.

When you enter information in Ruby all is treated as a string, if we insert numbers we need to convert them into it. To convert a string to an integer we can use the `to_i` method if instead you need a float you can convert it with `to_f`.

### Arrays
An array, that we generally store in a variable, is created using the `Array` keyword followed by the values that needs to contain.
```ruby
family = Array["Paola", "Andrea", "Allegra", "Giona"]
```
As for other languages, an array can store any kind of value. We can access specific indexes and, like seen for the strings, we can access a range of values passing a start and end index inside the `[]`.

`Array.new` creates a new array and allow us to add data later with the index syntax. `length` and `include` works like in the strings and syntax is pratically the same.

#### `reverse`
Will revert the order of the values in the array.

#### `sort`
Sorts the data in the array, but throw an error in case values contained are not of the same type.

### Hashes
A has allows us to store multiple information with a key/value syntax, like the objects in JS.

Like for a literal obj in JS we can create the hash with the curly braces:
```ruby
states = {
	"Pennsylvania" => "PA",
	"New York" => "NY",
	"Oregon" => "OR"
}
```
To access a value at a specific key we can do it passing the `key` inside brackets:
```ruby
puts states["Oregon"] // Prints OR
```

### Methods/Functions
When we define a method we use the keyword `def` and, unlike JS, we do not use curly braces. We end the method with the keyword `end`:
```ruby
def sayhi
	# The body of the method
end
```
Invoke the method is as simple as just call by it's name, but if it accepts parameter the syntax is a bit different;
```ruby
def sayhi(name)
	puts ("Hello " + name)
end

sayhi("Andrea")
```
We can pass default parameters too:
```ruby
def sayhi(name = "Andrea")
	puts ("Hello " + name)
end
```
As in many other fn like behavior in programming Ruby has the `return` operator:
```ruby
def cube(num})
	num * num * num
end
```
In this example I show that Ruby automatically returns the last instruction in the method, but I can give precedence with `return`. Using it allows us to also return different types of values:
```ruby
def cube(num})
	num * num * num, "Andrea"
end

puts cube(3)[1] # Prints Andrea
```

### If statements
The syntax, as already seen in the methods, is pretty different from JS but it still is a conditional statement.

```ruby
ismale = true

if ismale
	puts "You are male"
end
```
As you can see an `if` statement in Ruby does not uses parenthesis at all and we close the condition block in the same way we close methods, with an `end`. A condition can be a concatenation of single conditions and we use the keywords `and` and `or` at the place of `&&` and `||`
```ruby
ismale = true
istall = false

if ismale and istall
	puts "You are male AND tall"
elsif ismale and !istall
	puts "You are an ok make"
else
	puts "Well, you're ok"
end
```
> We negate a value with a `!` like in JS.

#### Comparison
We already saw some comparison with boolean with the previous code blocks but we can compare even data types and the comparison operator are equal in JS.
```ruby
def max(n1, n2, n3)
	if n1 >= n2 and n1 >= n3
		return n1
	elsif n2 >= n1 and n2 >= n3
		return n2
	else
		return n3
	end
end
```

### Case expression
This is like the `switch` in JS but the syntax is pretty different
```ruby
def get_day_name(day)
	day_name = ""
	
	case day
	when "mon"
		day_name = "Monday"
	when "tue"
		day_name = "Tuesday"
	# Other when...
	else
		day_name = "Invalid"
	end
	return day_name
end
```
As in previous cases we use a lot of English. We use `when` in place of `case` and `else` instead of `default`.
### For loop
The first logic is really similar to the one of `for...in`.
```ruby
for item in items
 # Other code...
end
```
But it offers a different syntax
```ruby
items.each do |item|
 # Other code...
end
```
`item` will be the variable that is contained but we can do even more:
```ruby
for num in 0,5
 # Other code that's looped 6 times
end

6.times do |num|
 # Other code that's looped 6 times
end
```
Using the `times` method the variable I put in the pipes will refer to the index of the loop.

If I want to range the loop within something I can even write them down like:
```ruby
(2..5).each do |num|
	puts "number is #{num}"
end

# Will prints
number is 2
number is 3
number is 4
number is 5

# 0 => 10 step size of 2
0.step(10,2).each do |range|
	puts "range is now #{range}"
end
```