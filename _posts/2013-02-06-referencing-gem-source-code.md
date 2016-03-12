---
layout: post
title: Referencing Gem Source Code
date: '2013-02-06 05:11:46 -0800'
categories:
- Ruby on Rails
- Gems
tags:
- gem
- gem source
- unpack
comments: []
---
It's often difficult to work with Ruby Gems that your Rails application depends on because the source code for the gem itself is packed away in a gem directory. I've often found myself using the command 'rvm gemdir' to output the path to the gem directory that my application is using, changing to that directory, and opening the source using Textmate. This is a time consuming process.

Instead, it's useful to simply unpack a gem into your Rails application so that it loads from the vendor/gems directory. I'm currently using the following command to unpack the RefineryCMS gems into my Rails app for reference.

``` shell
gem unpack refinerycms --version 2.0.9 --target vendor/gems
gem unpack refinerycms-core --version 2.0.9 --target vendor/gems
gem unpack refinerycms-dashboard --version 2.0.9 --target vendor/gems
```

These unpacked gems are not referenced in my Gemfile, so I expect there shouldn't be any conflicts. The source is just there for my referencing pleasure.

 

