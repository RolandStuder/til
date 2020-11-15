# Using `intance_eval` to provide easy to write procs

Instead of passing a context variable into a lambda or proc, we can use `instance_eval` instead

With a context variable it looks like this:
```ruby
class User
  def greet_with(greeting)
    greeting.call(self)
  end
end
hello = -> (user) { "Hello #{user.name}"}`

User.first.greet_with(hello)
````

With an instance_eval we can do:
```ruby
class User
  def greet_with(&greeting)
    instance_eval(&greeting)
  end
end
hello = proc { "Hello #{name}"}
```

It needs to be a proc and cannot be a lambda as far as I understand. 
As instance_eval passes `self` as an addtional argument into the proc, lambdas will raise an error,
as they are strict about the number of arguments provided.
