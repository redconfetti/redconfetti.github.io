---
layout: post
title: Building a Query String from a Hash with Rails 3
date: '2011-10-19 14:35:09 -0700'
comments: true
categories:
- Controller
---

I have a model has a method that generates and stores a cached link to it's
own view for use in mailers. Under Rails 2 the method which generated this
link created the beginning of the URL based on the owner of the object (this
is a multi-domain system I'm working on), however it required that a hash of
parameters be included in the link.

Under Rails 2 the 'options' hash would be passed to build_query_string inside
of my method like so:

``` ruby
params = ActionController::Routing::Route.new.build_query_string(options)
```

<!--more-->

Under Rails 3 I receive this error:

```ruby
wrong number of arguments (0 for 7)
```

It turns out that under Rails 3 the Hash library includes a to_query method.

``` ruby
irb(main):001:0> h = {:blah => '1', :blah2 => '2'}
=> {:blah=>"1", :blah2=>"2"}

irb(main):002:0> h.to_query
=> "blah2=2&amp;blah=1"
```

Thanks to mjrussel for [posting this on StackOverflow][1].

[1]: http://stackoverflow.com/questions/3576574/constructing-url-parameters-in-rails-3
