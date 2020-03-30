---
layout: post
title: Rspec Executable Not Found
date: '2012-11-28 01:14:27 -0800'
categories:
- Gems
tags:
- rvm
- rspec
comments: []
---
I'm working on an older Rails 2.3.8 application that is way too complicated and
without tests to make it worth upgrading to Rails 3 or higher. Because of this
we must use RSpec-Rails 1.3.4, with RSpec 1.3.2.

I was just trying to run a single test from the command line like so:

``` shell

$ bundle exec rspec spec/models/post.rb

bundler: command not found: rspec

Install missing gem executables with `bundle install`

```

I tried to uninstall and reinstall the gems, but still it didn't work. I am
using RVM, so this might be part of why this command isn't working.

It turns out that older versions of RSpec used 'spec' instead of 'rspec' as the
executable name.

``` shell

$ which spec

/Users/jsmith/.rvm/gems/ruby-1.8.7-p371@myproject/bin/spec

```
