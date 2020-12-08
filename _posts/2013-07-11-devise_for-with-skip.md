---
layout: post
title: Devise_For with Skip
date: '2013-07-11 22:26:51 -0700'
comments: true
categories:
- Ruby on Rails
tags:
- devise
---

I just stumbled upon the options for devise_for which let you auto-generate the
routes that are needed for a certain devise resource (user), with certain
categories of routes skipped.

For example, if I want to define routes for my User, I can define:

``` ruby
devise_for :users
```

This results in the routes for these three categories:

* Sessions - Sign in, Sign out
* Passwords - Password reset options
* Registrations - Creating new user, updating existing user, or destroying your
user account

You can leave one of these categories out of the route definition by using skip.
For instance if you want only the Sign-in and Sign-out options, you could define
this in your routes.rb file:

``` ruby
devise_for :users, :skip => [:registrations, :passwords]
```
