---
layout: post
title: Establishing New Ruby Environment in a Folder using RVM
date: '2012-05-15 17:58:16 -0700'
comments: true
categories:
- Ruby on Rails
tags:
- rvm
comments: []
---

I know this is documented on the [official RVM website][1], but I hate having to
look it up over and over again each time I want to create a new RVMRC file.

``` shell
$ mkdir -p ~/projects/rails2test
$ cd ~/projects/rails2test
$ rvm --rvmrc --create 1.8.7@rails2test
$ cd ..
$ cd rails2test
==============================================================================
= NOTICE                                                                     =
==============================================================================
= RVM has encountered a new or modified .rvmrc file in the current directory =
= This is a shell script and therefore may contain any shell commands.       =
=                                                                            =
= Examine the contents of this file carefully to be sure the contents are    =
= safe before trusting it! ( Choose v[iew] below to view the contents )      =
==============================================================================
Do you wish to trust this .rvmrc file? (/Users/jmiller/Documents/rails2-apps/.rvmrc)
y[es], n[o], v[iew], c[ancel]> y

```

<!--more-->

I'm needing to setup a Rails 2.3.8 system, so I can test my gem for
compatibility between it and Rails 3.0.9.

I stumbled onto [an article][2] with suggestions for how to install RubyGems
for Rails 2.3.8. This seems to run with errors, but the last command seemed to
complete without errors other than 'README' not found:

``` shell
$ rvm all do gem install -v 1.4.2 rubygems-update
$ rvm gem update --system 1.4.2
$ update_rubygems
$ rvm all do gem install -v 2.3.8 rails
$ rails --version
Rails 2.3.8
$ ruby --version
ruby 1.8.7 (2012-02-08 patchlevel 358) [i686-darwin10.8.0]
```

[1]: https://rvm.io/workflow/rvmrc/
[2]: http://ecmanaut.blogspot.com/2011/09/running-old-rails-238-with-rvm.html
