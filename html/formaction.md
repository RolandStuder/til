# Have buttons submit to different endpoints with HTML

So you have a blog post form, and you want to let the user either use one button to "Save as Draft", and another to "Save and Publish". I always thought you had to do conditionals using the `params[:commit]` value in Rails, or reach for a hidden field and some JavaScript.

Turns out HTML5 has you covered natively with `formaction` and `formmethod` — button-level overrides of the form's own action and method.
```erb
<%= form_with model: @blog_post do |f| %>
  <%# ... fields ... %>
  <%= f.submit "Save Draft", formaction: blog_post_path(@blog_post), formmethod: :patch %>
  <%= f.submit "Publish", formaction: blog_post_publication_path(@blog_post), formmethod: :post %>
<% end %>
```

This lets you keep a clean RESTful separation — drafts go to `BlogPostsController#update`, publishing goes to `BlogPost::PublicationsController#create`. No flag-sniffing in the controller needed.

A couple of things worth knowing:

- Always define an `action` on the form itself as the fallback — if the user hits Enter, the browser submits via the form's action using the first submit button in the DOM, not any `formaction`. So button order matters and an explicit form action avoids surprises.
- `formnovalidate` is in the same family and worth knowing about — it lets you skip browser HTML5 validation on a per-button basis, handy for a "Save Draft" button where incomplete forms should be allowed through.
- Turbo respects both attributes out of the box for normal clicks.
