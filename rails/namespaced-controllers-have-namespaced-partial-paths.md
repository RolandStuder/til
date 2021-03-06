# Partial paths for resources are depending on the namespace of the controller

In rails we can render a resource like this:

```erb
<%= render @post %>
```

It will look for the partial to render the resource in `wines/_wine.html.erb`.

**But** if you do a render from a namespaced controller, like `Admin::PostsController`.
It will instead look for `admin/wines/_wine.html.erb`.

So you can just use `render @record` in different places but have it render differently
depending on your namespace.
