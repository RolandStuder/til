# Run a method for each element in an enumerable, with that element as an argument.

One can use `map` or `each` on an enumerable, to call a method on each object:

```ruby
users.map(&:name)
```

But sometimes you want have a method that you want to call with each element in an enumerable, like

```ruby
users.each { |user| notifiy(user) }
```

I always wondered if there was not some way to write this differently, since you can do `things.map(&:something)`
should there not be away to call a method with each iteration, and actually there is:

```ruby
users.map(&method(:notifiy))
```

But honestly it is not very readable, and only margiinally shorter to write. If you feel lazy, I would
probably prefer to use this:

```ruby
users.map { notify(_1)}
```

But even that feels not really worthy.
