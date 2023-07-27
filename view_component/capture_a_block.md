# Capture blocks for view component with the viewcontext

When you are creating our own slot like configuration methods for [view components](https://viewcomponent.org), 
there is the issue tha if you call a block it will only return the last thing returned
by the block, so if you want to do this:

```erb
<%= ListComponent.new(items: @posts) do |list| %>
  <% list.display do |post|  %>
    <h1><%= post.title %></h1>
    <p><%= post.body %></p>
  <% end %>
<% end %>
```

And you component ruby file looks like this:

```ruby
class ListComponent < ViewComponent::Base
  def initialize(items:)
    @items = items
  end

  def display(&block)
    @display = block
  end

  def before_render
    content # ensure that block is called
  end
end    


If you know call this block in your component template
```erb
<%= @items.each do |item|
  <%= @thing_to_capture.call(item) %>
<% end %>
```
It will only output `</h1>` as this it the last thing the block returns.

In order to get everything within that block you need to use the viewcontext.

```erb
<%= @items.each do |item|
  <%= viewcontext.capture item, &@thing_to_capture %>
<% end %>
```

This will use the viewcontext to capture all the outputed strings in that block, any arguments you give to capture, will be
yielded to the block.

