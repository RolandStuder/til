# Abstract classes

Not sure where to use them, I guess if you have a library and want to communicate to someone 
which methods he has to implement in his class to make it work, you could provide an abstract class

But after reading this post on [Ruby Abstract Classes by Conner Jensen](https://www.connerjensen.com/blog/ruby-abstract-class/)
and reading
[this comment, suggesting to solve this with some metaprogramming](https://www.reddit.com/r/ruby/comments/k0zgu6/i_wrote_a_post_to_better_understand_abstract/gdme7xy/) 
on reddit,  I got curious on how I would implement this.

I came up with the follwing implementation:

```ruby
require 'active_support/concern'

module AbstractClass
  extend ActiveSupport::Concern

  class_methods do
    def abstract_instance_methods(*names)
      names.each do |name|
        define_method name, not_implemented
      end
      @@_abstract_instance_methods = names
    end

    def abstract_class_methods(*names)
      names.each do |name|
        define_singleton_method name, not_implemented
      end
      @@_abstract_class_methods = names
    end

    def unimplemented_methods
      @@_abstract_class_methods.select do |method_name|
        method(method_name).source_location == not_implemented.source_location
      end
    end

    def not_implemented
      proc { raise NotImplementedError }
    end
  end

  def unimplemented_methods
    @@_abstract_instance_methods.select do |method_name|
      method(method_name).source_location == self.class.not_implemented.source_location
    end
  end
end
```

You could then have an abstract class like:

```ruby
class SomeAbstractClass
  include AbstractClass

  abstract_instance_methods :instance_one, :instance_two
  abstract_class_methods :class_one, :class_two
end
```

And then your "ConcreteClass" would be something like:

```ruby
class ConcreteClass < SomeAbstractClass
  def self.class_one; end

  def instance_one; end
end
```

Now

```
ConcreteClass.new.instance_two # => NotImplementedError
ConcreteClass.new.unimplemented_methods # => [:instance_two]

ConcreteClass.class_two # => NotImplementedError
ConcreteClass.unimplemented_methods # => [:class_two]
```
