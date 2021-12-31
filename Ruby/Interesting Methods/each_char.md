Useful when you want to loop over the characters of a string. So next time you need to loop over a string, instead of separating each character with a `scan` or any other useful method you can just use this one:
```ruby
"new string".each_char do |char|
	# work on each char
end
```
