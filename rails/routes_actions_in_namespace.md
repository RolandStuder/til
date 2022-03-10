# In your `routes.rb` you can define `:only` for namespaced resources


```ruby
  namespace :website, path: nil, only: :show do
    resource :home, path: ""
    resource :story
    resource :wines
  end
  ```
  
  You `only` delcaration will be applied to the resources in the namespace, they will by default only get the :show action
  
