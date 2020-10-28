# Using form helpers in reflexes to enable use of existing partial that are part of a form.

So if you have a big form and you split it up into partials, you may still want to use the form context.
But for my partial full of `f.text_field :name` where do I get this damn f from?

We can just include the helpers in our Reflex and use fields_for

```ruby
include ActionView::Helpers::FormHelper
include ActionView::Context

def some_reflex
  html = fields_for(:test) do |f| ApplicationController.render(partial: "home/button", locals: {f: f}) end
  morph "#some-id", html
end
```
