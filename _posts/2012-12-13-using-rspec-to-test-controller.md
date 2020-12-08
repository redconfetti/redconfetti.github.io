---
layout: post
title: Using Rspec to Test Controllers
date: '2012-12-13 01:46:41 -0800'
categories:
- Testing
tags:
- rspec
- controller
comments: []
---

Here are some tips that will help you with Controller tests in Rspec.

## Common Response Methods

``` ruby
# Get HTTP response code with message. Example: "302 Found"
response.status

# Get HTTP response code. Example: 200
response.response_code

# Get response body
response.body

# Get location header, used with redirects
response.location
```
<!--more-->
## Common Matchers

``` ruby
# Check for successful response, same as response.success?.should be_true
response.should be_success

# Check if a specific template was rendered
response.should render_template("edit")

# Test if the controller method redirects to a specific path/url
response.should redirect_to(posts_url)

# Check a global variable assigned in the controller method
assigns(:owner_id).should eq(current_user.id)
```

## Mocking or Stubbing Partials

In this example I'm using Mocha with Rspec v1.3.2. I refer to the controller
itself in this Controller test as 'controller'. The 'render' or
'render_to_string' methods are part of the controller itself. In this example
it's rendering a partial to a string, and including it in a JSON hash being
returned to an AJAX call.

``` ruby
# Controller method uses render_to_string to render a partial to HTML string, includes in JSON response

controller.expects(:render_to_string).with(:partial => 'comment_block', :locals => {:post => post}).returns("comment block content").at_least_once
```

It's not advisable that you use a helper directly inside of a controller, and
thus you shouldn't need to stub one from within a controller method spec.
Helpers should be used within views, otherwise your "helper" method should exist
in a model, or in a utility library in /lib, so it's available in the controller
or elsewhere.

Although this post isn't on View testing, [this article][1] helps explain how to
mocking partials and helpers in views.

[1]: http://jakescruggs.blogspot.com/2007/03/mockingstubbing-partials-and-helper.html
