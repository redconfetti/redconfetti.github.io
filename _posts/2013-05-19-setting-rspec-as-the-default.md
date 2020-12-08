---
layout: post
title: Setting Rspec as the Default
date: '2013-05-19 05:19:49 -0700'
comments: true
categories:
- Ruby on Rails
tags:
- generators
- workflow
comments: []
---

When setting up a new Rails application you'll likely want to make Rspec the
default test framework for new models that are generated with scaffolding. This
is usually handled by default by the Rspec gem after you install it. It's
possible to explicitly set this however, as well as other configurations for generators.

This is explained in more details in the [Customizing Your Workflow] section of
the Rails generator documentation. You even have the option of turning off the
generation of stylesheets if preferred.

[Customizing Your Workflow]: http://guides.rubyonrails.org/generators.html#customizing-your-workflow
