# Creating a generator is quite simple

To create a Generator you just need a file like

```ruby
require "rails/generators"

module Book
  class ClassficationGenerator < Rails::Generators::Base
    desc "Creates an book classification file"
    source_root File.expand_path("templates", __dir__)

    def copy_config_file
      copy_file "app/domain/books/classifications/new.rb"
    end
  end
end
```
You then need said file in `templates/app/domain/books/classification` (I think the path is relative).

You call the genrator with `rails generate book:classification`. Where it is _module_name_:_class_name_without_generator_

This is kind of a useless example as for such a case, you would want to use an argument for the name of the
classification, that would be used for the file name as well as the class name. But that is for another day.


