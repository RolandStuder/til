# Conditional caching

While developing I often think: "Wouldn't it be handy, if Rails could…", and more often than not it **can**!

So I had this list of products that I wanted to cache, problem was, the page also should show the amount of items in the cart.
So caching it completely would be problematic. To still get the benefit of the caching the full fragment for the 99% of cases
where the user doesn't have the product in its cart, I looked at conditional caching. So I was just able to do:

```erb
<%= cache_unless(current_order.in_cart?(@product),  @product) do %>…
```

There is of course also a `cache_unless`
