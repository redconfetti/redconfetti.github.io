---
layout: post
title: Obtaining Request Domain Name for Ruby on Rails
date: '2010-10-12 01:19:16 -0700'
comments: true
categories:
- Ruby on Rails
tags: []
comments: []
---

I'm using Rails 2.3.8. To obtain the domain name for the website being
requested (i.e. mysite.com, mysite.net), just reference 'request.host'.

```ruby
ruby@host = request.host
```

You can only reference request.host in the views, or the controller.
