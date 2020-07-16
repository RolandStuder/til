# Use global ids to make generic reflexes

Say you want to delete an object in a reflex that is not specific to any view or model. You could write something like:

```erb
<button data-reflex="click->deletable#delete" 
        data-reflex-id="<%= model.id %>"
        data-reflex-model-class="<%= model.class.name %>> 
```

And then in the reflex

```ruby
def delete
  id = this.element.dataset[:id]
  model_class = this.element.dataset[:model_class].constantize
  model_class.send(:delete,id)
end
```

Now this works, but you can have to pass the model class every time, with global ids we won't need to.

## About global (signed) ids

[Rails provides methods](https://github.com/rails/globalid) on ActiveRecord models to get global ids independent of the model class:

```ruby
model.to_gid.to_s # gives a global id
model.to_sgid.to_s # gives you global signed id

GlobalID::Locator.locate some_id # returns a record from an id
GlobalID::Locator.locate_signed person_gid # returns a record from a signed id.
```

## Make you reflex call simpler

Now with global ids we don't have to pass the model class anymore to the reflex:

```erb
<button data-reflex="click->deletable#delete" 
        data-reflex-sgid="<%= model.to_sgid.to_s %>"
```

```ruby
def delete
  record = GlobalID::Locator.locate_signed element.dataset[:sgid]
  record.delete
end
```
