# Redirects will strip anchor / fragments when redirecting with Turbo

Given you redirect in a controller like this:

```ruby
  def update
    post = Post.find(params[:id])
    if post.update(post_params)
      redirect_to posts_path(anchor: ActionView::RecordIdentifier.dom_id(post))
    end
  end
```

It will give you a redirect path of `/posts#post_1`. However the browser will strip the fragement from the url.
So the browser won't scroll to the anchor tag.

This is a limitation of how `fetch` is implemented by browser, so Turbo can't actually fix this. [More info in this Turbo issue](https://github.com/hotwired/turbo/issues/211)
