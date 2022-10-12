# Use `instance_variable_defined?(:@var)` when memoizing potentially falsy variables

For memoization one can usually use an `||=`, however that does not work for falsy values like:

```ruby
def thingy
  @thingy ||= true
end
```

If one set `@thingy` to false, it will be set to `true` on calling the getter.

Instead one can use:

```ruby
def thingy
  unless instance_variable_defined? :@thingy
    @thingy = true
  end
  
  @thingy
end
```
