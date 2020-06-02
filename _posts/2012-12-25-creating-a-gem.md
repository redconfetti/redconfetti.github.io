---
layout: post
title: Creating a Gem
date: '2012-12-25 09:50:03 -0800'
categories:
- Gems
tags:
- gem
comments: []
---
In the past gems were created manually, or generated using the [echoe gem] (last
release Sept 21, 2011), or the [Jeweler gem] (last release November 7, 2011).

Since then it appears that the most automated way to create a gem is by using
Bundler, via the `bundle gem` [command].

[echoe gem]: http://rubygems.org/gems/echoe
[jeweler gem]: https://github.com/appoxy/jeweler
[command]: http://gembundler.com/v1.2/bundle_gem.html

```ruby
$ bundle gem my_tools
      create  my_tools/Gemfile
      create  my_tools/Rakefile
      create  my_tools/LICENSE
      create  my_tools/README.md
      create  my_tools/.gitignore
      create  my_tools/my_tools.gemspec
      create  my_tools/lib/my_tools.rb
      create  my_tools/lib/my_tools/version.rb
Initializating git repo in /Users/jsmith/Sites/my_tools
```
