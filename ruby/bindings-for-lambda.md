# What binding lambdas have…

## What is `binding`?

The binding is something like the context in which we currently are in the code. Or as the ruby documentation says:
> Objects of class Binding encapsulate the execution context at some particular place in the code and retain this context for future use.
[Class: Binding](https://ruby-doc.org/core-2.7.2/Binding.html)

## What does this matter for lambdas?

If I write a lambda, it matters where I write it.

```ruby
class User
  MY_LAMBDA = -> { some_class_method } 
  # will have the class as context, so class methods are callable
  def foo
    @foo ||= lambda { some_instance_method } 
    # will have an instance as context, so instance methods are available
  end
end
```

## What do I care for `binding`?

Well I really didn't until I wanted to help 
[provide a configurable logger for StimulusReflex](https://github.com/hopsoft/stimulus_reflex/pull/337)

The question was, how to we monkey patch string to provide easy colors methods like `"hello".red`
without changing the string class globally.

You can use `refine` for this, but how do we get this into the context of the lambda the user provides?

Well there you go: You take the `binding` and you do stuff in it:
```ruby
config.logging.binding.eval("using Colorize")
