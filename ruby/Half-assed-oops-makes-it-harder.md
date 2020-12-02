# Half assed OOP doesn't work.

For AdventOfCode I had to make a [Password Policy Checker](https://adventofcode.com/2020/day/2). But I messed the responsibilities up, I had:

```ruby
Password.new(policy_string, password_string)
```
My idea to inject the policy was ok, but really, if I just inject the string, then I need a seperate argument for the policy.

```ruby
Password.new(policy_string, password_string, PolicyOne)
```

But actually I should have just left the Policy itself figure things out:

```ruby
PasswordPolicy.valid?(policy_string, password_string)
```

You could then still do

```ruby
class Password
  def valid?(policy: PasswordPolicy)
    policy.valid?(policy_string, password_string)
  end
 end
```
Or probably, the password class should never know about the `policy_string`, 
which is why `PasswordPolicy.valid?(policy_string, password_string)` is 
the best approach I think
