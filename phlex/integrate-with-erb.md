# Integrate Phlex with erb

You can easily create something like slots like this:

## In phlex

````ruby
class ErbSlotComponent < Phlex::HTML
  def template
    yield
    div(class: "bg-gray-200 rounded p-4") do
      @slot.call
    end
  end
  
  def slot(&block)
    @slot = block
  end
end
````

## In ERB

```erb
<%= render ErbSlotComponent.new do |component| %>
  <% component.slot do %>
    <%= link_to :root, :root %>
  <% end %>
<% end %>
```

## Use phlex provided methods in the block

You can use component methods in the block, like so:

```erb
<%= render ErbSlotComponent.new do |component| %>
  <% component.slot do %>
    <%= component.h2 :my_title %>
    <%= component.some_method_that_does_fancy_rendering :foo %>
  <% end %>
<% end %>
```

## Make the phlex view be the context of the block

To make methods directly available, without having to use `component.some_method`, we can use lambdas

```ruby
class ErbSlotComponent < Phlex::HTML
  def template
    yield
    div(class: "bg-gray-200 rounded p-4") do
      instance_exec(&lambda)
    end
  end
  
  def from_lambda(lambda)
    @lambda = lambda
  end
end
```

Which demands something like this:

```erb
<% local_variable_from_erb = "hi from erb" %>
<%= render ErbSlotComponent.new do |component| %>
  <%
    component.from_lambda -> do
      div { local_variable_from_erb }   
    end
  %>
<% end %>
```

Which is not terrible, but definitely a bit unusual. However probably an antipattern anyway.
