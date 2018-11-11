---
layout: post
title: Custom Rake Tasks Not Loading
date: '2012-01-01 18:57:34 -0800'
categories:
- Ruby on Rails
tags:
- rake
---

I recently went to create a new Rake task under /lib/tasks in a Rails
application I'm working on. I didn't understand why the rake tasks weren't
showing when I would run `rake -T` from the command line.

When I'd try to run the task itself I would get a 'Don't know how to build
task' error.

I just realized that I was naming my task file with the '.rb' extension, and
not '.rake'. Doh!
