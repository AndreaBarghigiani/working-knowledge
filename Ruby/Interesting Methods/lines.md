# Definition
`lines(separator=$/, chomp: false) → an_array`

## Thinkings
Really useful when you need to split a string based on a separator, it will return an array.

It also accepts a `chomp` attribute that, if defined, will remove empty space from string in array.

This methods is really similar to `split` but it is entirely focused on looping a lines based on a specific separator. With `split` instead you can split a string by any kind of pattern and the default one is a space  .

## Discovered
Found this method while doing the Matrix on Exercism from a community solution and used like so:
```ruby
text.lines.map { |l| l.split.map(&:to_i) }
```