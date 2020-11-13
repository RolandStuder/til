# Use `refine` to monkey patch selectively

Let's say we want to monkey patch some core class like string to enable coloring.
We can it like that:

```ruby
class String
  def blue
    "\e[34m#{self}\e[0m"
  end
end
```

However this way we have no control over where this is available.
And if I use it in a Gem, I would not want to surprise people with
random methods in a core class.

So instead we create a module and `refine` the `String`class:

```ruby
module Colorize
  refine String do
    def blue
      "\e[34m#{self}\e[0m"
    end
  end
end
```

No we can do:

```ruby
using Colorize
"I am blue".blue
```
