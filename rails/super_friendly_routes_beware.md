# Super friendly routes are possible, but beware

[Rails routing](https://guides.rubyonrails.org/routing.html) provides a lot of power. 
Coupled with the [friendly_id gem](https://github.com/norman/friendly_id/) you can get really nice semantic urls.

So you can have a resource like this:

```ruby
# routes
get "/:id, to: "profiles#show"
```

But if you want to have several resources unter the top path then you get into trouble.

```ruby
# routes
resource :shops, path: "/"
get "/:id, to: "profiles#show"
```

Now the first route will always match, whether the slug is for a shop or a profile. So the `ShopsController` will take over and
throw a `RecordNotFound` error.

So what can we do then:

## Option 1: Have a specific slugs controller, that forwards to the right controller:

see here: https://hackernoon.com/root-level-slugs-for-multiple-controllers-with-friendlyid-1a4a7ba03ec1

## Option 2: Use a route constraint:

```ruby
resources :shops, path: "/", constraints: lambda { |request|
                                                      FriendlyId::Slug.exists?(slug: request.params[:id],
                                                      sluggable_type: "Shop")
                                                 }
```

Notes: Slugs are maybe not  nique between the two models. So there is a lot to consider here. 
Check if it's acutally worth it having multiple resources directly under the root url with a friendly id.
Especially since urls like those are not something you want to have to change later, or mess up.

