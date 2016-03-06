---
layout: post
title: Recommended Sublime 3 Packages
categories:
- Ruby
tags:
- ruby
---
The documentation for [alias_method_chain](http://apidock.com/rails/Module/alias_method_chain) seems to be lacking, so I figured I'd think aloud here and perhaps end up providing a little clearer explanation.

We have to start with [alias_method](http://apidock.com/ruby/Module/alias_method), which is explained using this example code:

``` ruby
module Mod
  alias_method :orig_exit, :exit
  def exit(code=0)
    puts "Exiting with code #{code}"
    orig_exit(code)
  end
end

include Mod
exit(99) # => Exiting with code 99
```

So the first argument is the name of the method you want to define, and the second argument is the method you want to copy the behavior of.

Alias method chain is meant to replace this pattern, it says:

``` ruby
alias_method :foo_without_feature, :foo
alias_method :foo, :foo_with_feature
```

So it creates a new method called #foo_without_feature from #foo. I guess I have to assume that #foo doesn't have some sort of feature.

Then it creates a new method called #foo from #foo_with_feature. I guess one would need to define #foo_with_feature first? I think we're missing some sort of context here.

``` ruby
alias_method_chain :foo, :feature
```

The implication here is that the above call creates #foo_without_feature and makes #foo copy from #foo_with_feature.

``` ruby
#!/usr/bin/env ruby

require File.expand_path('../../config/boot',  __FILE__)
require RAILS_ROOT + '/config/environment'

module Mod

  module ClassMethods
    def foo_with_feature
      puts "Hi I'm Foo with Feature!"
    end
  end

  def self.included(base)
    base.class_eval do
      extend ClassMethods
      class &lt;&lt; self
        alias_method :foo_without_feature, :foo
        alias_method :foo, :foo_with_feature
      end
    end
  end

end

class Foo
  include Mod
  def foo
    puts "Hi I'm Foo!"
  end
end

new_foo = Foo.new
new_foo.foo

new_foo.foo_without_feature
```
