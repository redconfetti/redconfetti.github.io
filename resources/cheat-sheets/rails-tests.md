---
layout: page
title: Rails Tests
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

These may be helpful to some, but truthfully I highly recommend learning how to use [Rspec](http://rspec.info/) instead. The tests are much easier to read and write. You can use [Rspec Rails](https://github.com/rspec/rspec-rails) for Ruby on Rails projects, and have the ability to test your controllers separate from your views. It even has tests for routes, just in case you might need them.

``` shell
# Run Test Unit Test
ruby -Itest test/unit/post_test.rb

# Run Test Unit Test Method
ruby -Itest test/unit/post_test.rb -n test_the_truth

# Run a specific RSpec file
rspec spec/models/post_spec.rb

# Run a specific example group (using line number) in RSpec file
rspec spec/models/post_spec.rb:545

# Run a specific Cucumber feature
cucumber features/manage_posts.feature

# Run a specific Cucumber scenario (using line number) in feature file
cucumber features/manage_posts.feature:33
```
