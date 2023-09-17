---
layout: post
published: true
title: Updating RBEnv on Raspberry Pi
date: 2021-06-14 07:19:55 -0700
comments: true
categories:
  - ruby
tags:
  - raspberrypi
  - rbenv
---

Here's a modified version of instructions provided by [Yosei Ito]. I used
`apt remove` instead of `apt uninstall`.

<!--more-->

```ruby
sudo apt remove ruby-build
$ mkdir -p "$(rbenv root)"/plugins
$ git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
```

This worked very well for me, helping me to install Ruby 2.7.3, and install
the gem dependencies for this website without any errors or need for using
'sudo' when installing gems via Bundler.

[Yosei Ito]: https://lmlab.net/memo/2021/06/03/use-rbenv-on-raspberrypi.html
