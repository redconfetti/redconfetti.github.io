---
layout: post
title: Creating a Namespaced Generator in a Gem for Rails 3
categories:
- Ruby
tags:
- ruby
- namespacing
- gem
---
I'm working on setting up a gem for a Rails 3.0.9 system that provides Rails engine functionality. I just realized that if I want to establish a [custom logger](http://stackoverflow.com/questions/6233401/custom-logger-in-rails-3) for my gem, I'll need to [provide a generator](http://railscasts.com/episodes/218-making-generators-in-rails-3?view=asciicast) that creates an initializer in 'config/initialize' from a template I define in my gem. This initializer will be responsible for and defining the constant that represents the custom logger. I followed instructions to add a generator to my gem by creating 'lib/generators', and then attempting to setup my generator class under a subdirectory of that folder. I wasn't sure what naming conventions were required, so I just tried to mimic the generator folder structure of Devise since it uses an installation generator (similar to what I'm doing), and I also inspected the generator code for Factory Girl Rails.

``` shell
module CompanyName
  module GemName
    module Generators
      class InstallGenerator < Rails::Generators::Base

        source_root File.expand_path("../templates", __FILE__)
        desc "Creates an initializer for my custom logger."

        def copy_initializer
          template "gem_name.rb", "config/initializers/gem_name.rb"
        end

        def show_readme
          readme "README" if behavior == :invoke
        end

      end
    end
  end
end
```

The commands used within the generator class are much different in Rails 3 than they were in Rails 2, due to Rails 3 generators being based on [Thor](https://github.com/wycats/thor). Thor provides better options for parsing commands/arguments, and a [great API](http://rdoc.info/github/wycats/thor/Thor/Actions) for manipulating files. The generator class you define inherits from Rails::Generators::Base, which itself inherits from [Thor::Group](http://api.rubyonrails.org/classes/Rails/Generators/Base.html). I just realized that the current [Edge Guide on generators](http://edgeguides.rubyonrails.org/generators.html) is actually still relevant to Rails 3.0.9 even though it's likely meant for Rails 3.2.2\. I guess the API for generators haven't changed with Rails 3.1 like it did with Rails Engines. Anyway, I created my generator, built my gem, installed it in my app via Bundler using my local gem server as a source, and then ran 'rails generate'. My generator showed up like so:

``` shell
CompanyName
  company_name:gem_name:install
```

You'll notice that this includes my company name. That's because I'm wanting to ensure that my gem is operating within a namespace that doesn't conflict with other modules or classes in the future. The problem I had though was that I ran my generator, and I got the error: "Could not find generator company_name:gem_name:install". I was puzzled why this could happen. It sees my generator, and yet it can't find it when I try to run it. There must be some sort of convention issue going on here. So I instead moved the generator class under 'lib/generators/company_name/gem_name/install_generator.rb'. Re-installed my gem and tested again, and this time I got no error...however it didn't look like my methods are being called to copy the initializer file. At least I'm not getting the error that it's not found though. It looks like I've identified what is expected when your generator is namespaced...your generator file must be in a directory that is also namespaced. I know I keep discovering this with Rails, but I wish it was explicitly defined somewhere WHEN it's required.
