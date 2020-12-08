---
layout: post
title: RSpec Controller Tests Receiving 'No route matches' Error
date: '2012-05-25 18:24:17 -0700'
categories:
- Gems
tags:
- rspec
- namespaced controller
comments:
- id: 1051
  author: Bonyiii
  author_email: boni@twine.hu
  author_url: ''
  date: '2012-10-19 09:20:08 -0700'
  date_gmt: '2012-10-19 09:20:08 -0700'
  content: Thank you!
---

I'm developing a Rails engine gem for the company I'm working for, which will
provide an API for the applications we're using.  The gem I'm creating will be
used with a Rails 3.0.9 system, using Rspec-Rails version 2.10.1. I had a route
to my API interface setup in the config/routes.rb file like so:

``` ruby
Rails.application.routes.draw do
  match '/companyname/api_name' => 'CompanyName/ApiName/ControllerName#apimethod'
end
```

When I added a 'get' request call to my controller test, I was getting this
error:

``` shell
Failure/Error: get :apimethod
ActionController::RoutingError:
  No route matches {:controller=>"company_name/api_name/controller_name", :action=>"apimethod"}
```

<!--more-->
I spent a great deal of time trying to figure out how to get my test to work.
Different versions of Rspec, redefining my route using nested scopes, etc.

It turns out I just needed to redefine my route in underscore case so that
RSpec could match it with an existing route that was defined.

``` ruby
match '/companyname/api_name' => 'company_name/api_name/controller_name#index'
```

I guess Rspec controller tests use a reverse lookup based on underscore case,
and not camelcase). Rails will setup and interpret the route if you define it
in either case though.

Seems so simple now that I know the answer. Hopefully I'll save someone else
time with this post.
