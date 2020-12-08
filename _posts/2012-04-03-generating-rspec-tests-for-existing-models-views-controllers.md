---
layout: post
title: Generating Test File Stubs for Existing Models, Views, and Controllers
date: '2012-04-03 13:10:12 -0700'
categories:
- Testing
tags:
- rails3.1
- testing
- rspec
- tdd
---

I've noticed that if you install certain testing gems, like Factory Girl, or
Rspec, that your Rails application will create test files for these libraries
instead of using the defaults. Even further you can configure the generators
used by your Rails app in /config/application.rb

``` ruby
# Configure generators values. Many other options are available,
# be sure to check the documentation.
# http://edgeguides.rubyonrails.org/generators.html#customizing-your-workflow

config.generators do |g|
  g.stylesheets false
  g.test_framework :rspec
  g.fallbacks[:rspec] = :test_unit
  g.fixture_replacement :factory_girl
end
```

<!--more-->

I've been anxious however in deciding which testing tools to learn and use
with my project. If I choose the wrong one, then all the scaffold generated
test code will be generated for the test framework I might choose to quit
using at some point.

This is not completely correct though, as I've discovered.

I found that the following commands will generate the empty spec files.

``` shell
rails generate rspec:model

rails generate rspec:view

rails generate rspec:controller
```

Even better though, if you're looking for scaffold style files to be put in
place, use the following syntax to generate scaffold code.

``` shell
rails generate rspec:scaffold Post title:string body:text
```

If you decide to use basic Test::Unit based tests, instead of going with
Rspec, you can also reconfigure your app to use Test::Unit again with
generators, and then use the rake commands to generate the files that were
generated via rspec.

``` shell
$ rails g

Usage: rails generate GENERATOR [args] [options]

General options:

  -h, [--help]     # Print generator's options and usage
  -p, [--pretend]  # Run but do not make any changes
  -f, [--force]    # Overwrite files that already exist
  -s, [--skip]     # Skip files that already exist
  -q, [--quiet]    # Suppress status output

Please choose a generator below.

TestUnit:
  test_unit:controller
  test_unit:helper
  test_unit:integration
  test_unit:mailer
  test_unit:model
  test_unit:observer
  test_unit:performance
  test_unit:plugin
  test_unit:scaffold
```
