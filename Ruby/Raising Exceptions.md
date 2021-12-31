An exception is a form of error handling and are called like so because appear when dealing with some unexpected event.

We can raise an exception using the `raise` method and passing an object to it, normally an Exception object like `RuntimeError`.
```ruby
# These are equivalent
raise RuntimeError.new("Please implement this method")
raise RuntimeError, "Please implement this method"
raise "Please implement this method"
```

# Deep Dive
* https://rollbar.com/guides/ruby/how-to-raise-exceptions-in-ruby/
* https://www.honeybadger.io/blog/a-beginner-s-guide-to-exceptions-in-ruby/