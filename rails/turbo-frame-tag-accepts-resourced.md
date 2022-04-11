# Turbo frame tags can take a resource

Given you need to target a specific resource, like a contact, you might want to do something like this:

```ruby
<%= turbo_frame_tag dom_id(contact) do %>
```

But actually you just pass the resource to it directly:

```ruby
<%= turbo_frame_tag contact do %>
```
