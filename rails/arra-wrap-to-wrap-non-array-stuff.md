# Array.wrap will wrap something into an Array, unless it already is an array.

For when a API provider thingks its fine to deliver either an Object or an Array of Objects under the same key…

```ruby
Array.wrap(response.body[:some_thing_that_may_be_an_array_or_nor])
```

btw: This is a rails addition to Array, not otherwise available in Ruby
https://apidock.com/rails/v6.1.3.1/Array/wrap/class
