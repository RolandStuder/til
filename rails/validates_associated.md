# Validates associated only adds one error on the parent…

I got quite confused the other day I had a thing like:

```ruby
class Order < ApplicationRecord
  validates_associated :answers
end

class Answer < ApplicationRecord
  def some_validation
    errors.add(:base, :invalid)
  end
end
```

When doing `order.validate?` it triggered the answer validation and the Order record became invalid.
The error on ther Oder was: `attribute: :answers, type: :invalid`

Through circumsntatial things and a some indirection this confused me because the translation wer not picked up the way I expected them to.
But then I learned, that validates_associate, does not bubble up the errors. The error on the order (the parent) is merely the `attribute_name is invalid`.

You can translate that error messsage, but the interpolation for the translation has actual errors on the child, and you can't pass options in.
You need to handle, make them visible directly.

This might seem obvious in hindsight, but it had me going crazy, because another dev had interpoltation for the translation on that error, using `%{value}`,
which gives confusing result, because `value` is the collection of `answers`, as somehow validates_associated does soemthing like 
`errors.add(association_name, :invalid, value: send(association_name))`
and I kept being confused by trying to set the options for the error manually on the child record. And they got set, but of course the validation error
on the parent, did not show the options I set on the child.

Maybe this is not the most helpful TIL, it was an colleciton of things that came together that confused me completely.
And I felt like it was good to write down, how validates_associated actually behaves.

Sidenote: LLMs were not very helpful on this one.
