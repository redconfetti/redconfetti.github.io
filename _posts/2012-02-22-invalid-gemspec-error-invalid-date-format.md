---
layout: post
title: Invalid Gemspec Error Regarding Invalid Date Format
date: '2012-02-22 18:30:45 -0800'
categories:
- Ruby on Rails
tags:
- rails3.1
- rubygems
comments:
- id: 336
  author: Michael
  author_email: duananet@gmail.com
  author_url: ''
  date: '2012-03-15 06:36:30 -0700'
  date_gmt: '2012-03-15 10:36:30 -0700'
  content: |-
    Thanks,

    similar for me.

    sh-3.2$ cd /Library/Ruby/Gems/1.8/specifications/

    sh-3.2$ sudo find . -type f | sudo xargs perl -pi -e 's/ 00:00:00.000000000Z//'
---
I installed factory_girl (2.6.0) for a project I am working on recently, and all of a sudden I started getting errors with RubyGems when I would try to run a rake task, such as:

``` shell
Invalid gemspec in [/opt/local/lib/ruby/gems/1.8/specifications/capistrano-2.11.2.gemspec]: invalid date format in specification: "2012-02-22 00:00:00.000000000Z"

Invalid gemspec in [/opt/local/lib/ruby/gems/1.8/specifications/capistrano-2.9.0.gemspec]: invalid date format in specification: "2011-09-24 00:00:00.000000000Z"
```

I went through the long process of [uninstalling all my gems](http://geekystuff.net/2009/01/14/remove-all-ruby-gems/), running an update on RubyGems using 'gem update --system' and then reinstalling them again via 'bundle install'. Still the gems were uninstalled, yet the specification errors were still occurring.

I tried to run 'gem cleanup' or 'gem pristine --all' to get rid of the error. Eventually I was able to resolve the issue, but I don't remember exactly how I did it. Then today I go to work on another workstation, and the same thing occurs. It turns out that the invalid strings just need to be [removed from the specification files](http://paikialog.wordpress.com/2012/01/08/fixes-for-invalid-date-format-specification-in-gemspec/).

As the errors pointed to files in /opt/local/lib/ruby/gems/1.8/specifications/ (I'm using [MacPorts](http://www.macports.org/)), running these commands resolved the issue for me.

``` shell
cd /opt/local/lib/ruby/gems/1.8/specifications/

sudo find . -type f | sudo xargs perl -pi -e 's/ 00:00:00.000000000Z//'
```

