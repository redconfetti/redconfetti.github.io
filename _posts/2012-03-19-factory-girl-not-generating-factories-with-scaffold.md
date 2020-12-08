---
layout: post
title: Factory Girl Not Generating Factories with Scaffold
date: '2012-03-19 14:18:44 -0700'
categories:
- Testing
tags:
- rails3.1
- factory_girl
---

I just started a new Rails 3.2 project, and to ensure that the proper test
files are generated using Shoulda or Factory_Girl, I've installed those gems
and configured the application to generate the test files using these gems.

Added to config/application.rb:

``` ruby
  # Configure generators values.
  # http://guides.rubyonrails.org/generators.html
  config.generators do |g|
    g.stylesheets false
    g.test_framework :shoulda
    g.fallbacks[:shoulda] = :test_unit
    g.fixture_replacement :factory_girl
  end
```

Each time I would try to create a new scaffold, it would use shoulda to
generate the test unit file, but would generate a YAML fixture.
<!--more-->

``` shell
invoke active_record
create db/migrate/20120319180004_create_posts.rb
create app/models/post.rb
invoke shoulda
create test/unit/post_test.rb
create test/fixtures/posts.yml
```

Over a year ago there was [a gem needed][1] to ensure that generators were
present to generate Factory_Girl factories instead of YAML fixtures, but the
code for those generators was moved to the official Factory_Girl gem, so
that's not the cause of this issue.

It turns out that I had configured factory_girl_rails in my Gemfile only under
the 'test' group, and not the 'development' group as well.

``` ruby
group :development, :test do
  gem 'shoulda'
  gem 'factory_girl'
  gem 'factory_girl_rails'
end
```

After configuring these under both test and development, the scaffold
generator created the factory under 'test/factories' as I had expected.

[1]: https://github.com/indirect/rails3-generators
