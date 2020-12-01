# Form with submits with AJAX

I kinda new that `form_with` submits with Ajax, but I was quite confused, when 
Flashes were not working, neither with a `render` or a `redirect_to`.

Rails just does not handle that. So either you set your form to `local: true`
or you write some `.js.erb` to handle the response.
