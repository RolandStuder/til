# Painful learning: Do not open / require gem classes manually if you don't want Zeitwerk to fuck you up.

So I was using Lookbook and wanted to secure the lookbook controller to require some authentication:

basically I did:

```ruby
# /app/controllers/lookbook/application_controller.rb

gem_dir = Gem::Specification.find_by_name("lookbook").gem_dir
require "#{gem_dir}/app/controllers/lookbook/application_controller"

module Lookbook
  class ApplicationController < ActionController::Base
    before_action :authenticate_admin_user!
  end
end
```

That is a big no-go.
The manual require makes it work on the initial load. But as soon as any changes happen and Zeitwerk tries to reload the application, it will:
- reload the `Lookbook::ApplicationController`
- this only loads the class within our own application.
- therefore our class only has exactly that, it will not load the actual `Lookbook::ApplicationController` the gem provides, as Zeitwerk thinks you own that class

**So beware of reopening gem classes this way, it will mess with Zeitwerk, your "reopening" will be the only definition of the class when the application reloads via Zeitwerk"**

Workaround if you must somehow reopen the class and have no other option:
- ignore this file for the autoloaders: `Rails.autoloaders.main.ignore(Rails.root.join("app/controllers/lookbook"))`
- try to solve it differently first thoug, something like a constraint in the routes file for example: `constraints(-> req { req.env["warden"]&.user&.admin? }) { mount Lookbook::Engine, at: "/lookbook" }`

another option:

```ruby
# config/initializers/lookbook_guard.rb
Rails.application.config.to_prepare do
  next unless defined?(Lookbook::ApplicationController)
  Lookbook::ApplicationController.class_eval do
    before_action :authenticate_admin_user!
  end
end
```

So if you run into errors like:
- `undefined method `lookbook_render' for an instance of class…`
- `The action 'index' could not be found for Lookbook::ApplicationController`

That might be it.
