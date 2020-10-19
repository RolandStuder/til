# Use `ENV.fetch("SOME_VAR)` to get environment variables

While you can get environment variables like this `ENV["SOME_VAR]`, it might be better to use fetch.
The difference is that fetch will raise a `KeyError` if it is not defined. So you will now it is missing,
when you run the initializer and not only once the some code tries to use it.

So go for:
```ruby
ENV.fetch("SOME_VAR)`
```

btw: `ENV` is a hash-like accessor, so you can use methods like `.each`. `.keys`, `.select` and so on.
