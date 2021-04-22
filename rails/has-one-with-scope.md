# Turn things on its head with a has_one relationship with scope

Let's say you have a list of ordered products, that a customer can rate. Seems simple enough, but can get tricky.
One could think, a I just need a list of ratings, just find or initialize them for all the products. But that 
is kind of tricky. To sort your collection or dynamically updating item in the list, you will end up
having to use the product attributes to sort and you will need to use the dom_id of the product for partial updates.

So you will have a list of ratings, that is actually more a list of products. So why not just turn it on
its head? Let's get a list of ordered products and find or initialize its associated rating.

But how? I can do a `find_or_initialize_by(product_id: product.it)` on each product, but this will create a N+1 query. I really want
to avoid that. I just want have `ordered_products.includes(:rating)`. That to me seemed impossible, because such a relation
does not exist.

Turns out, you can create such a relation by using a has_one with a scope, that can be used for includes:

```ruby
# app/models/product.rb
has_one :rating, -> { where(user_id: Current.user.id)}
```

So there you have, it, now I can ask for `ordered_products.includes(:rating)`and in the view I can have:

```erb
<%= form_with(model: (product.rating || product.build_rating)) …
```

Very neat.

For testing purposes and more flexible use, it might make sense to make it possible to pass in a user.

```ruby
# app/models/product.rb
has_one :rating, -> (user_id = Current.user.id) { where(user_id: user_id)}
```
