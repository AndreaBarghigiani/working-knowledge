`:sym` this is the standard syntax to define a symbol but they're more powerful than that:
```ruby
"foo-bar (baz)".to_sym #> :"foo-bar (baz)" the space is kept
:"foo-bar (baz)" #> :"foo-bar (baz)" the space is kept

id = 5
:"foo-#{id}" #> :"foo-5" iterpolated value

:'hello, "world"' #> :"hello, \"world\"" escapes character

%s{foo:bar} #> :"foo:bar"
```