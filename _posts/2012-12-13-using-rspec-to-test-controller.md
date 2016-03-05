---
layout: post
status: publish
published: true
title: Using Rspec to Test Controllers
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1372
wordpress_url: http://www.rubycoloredglasses.com/?p=1372
date: '2012-12-13 01:46:41 -0800'
date_gmt: '2012-12-13 01:46:41 -0800'
categories:
- Testing
tags:
- rspec
- controller
comments: []
---
<p>Here are some tips that will help you with Controller tests in Rspec.</p>
<h2>Common Response Methods</h2></p>
<pre class="brush:rails">
# Get HTTP response code with message. Example: "302 Found"<br />
response.status</p>
<p># Get HTTP response code. Example: 200<br />
response.response_code</p>
<p># Get response body<br />
response.body</p>
<p># Get location header, used with redirects<br />
response.location<br />
</pre></p>
<h2>Common Matchers</h2></p>
<pre class="brush:rails">
# Check for successful response, same as response.success?.should be_true<br />
response.should be_success</p>
<p># Check if a specific template was rendered<br />
response.should render_template("edit")</p>
<p># Test if the controller method redirects to a specific path/url<br />
response.should redirect_to(posts_url)</p>
<p># Check a global variable assigned in the controller method<br />
assigns(:owner_id).should eq(current_user.id)<br />
</pre></p>
<h2>Mocking or Stubbing Partials</h2></p>
<p>In this example I'm using Mocha with Rspec v1.3.2. I refer to the controller itself in this Controller test as 'controller'. The 'render' or 'render_to_string' methods are part of the controller itself. In this example it's rendering a partial to a string, and including it in a JSON hash being returned to an AJAX call.</p>
<pre class="brush:rails">
# Controller method uses render_to_string to render a partial to HTML string, includes in JSON response<br />
controller.expects(:render_to_string).with(:partial => 'comment_block', :locals => {:post => post}).returns("comment block content").at_least_once<br />
</pre></p>
<p>It's not advisable that you use a helper directly inside of a controller, and thus you shouldn't need to stub one from within a controller method spec. Helpers should be used within views, otherwise your "helper" method should exist in a model, or in a utility library in /lib, so it's available in the controller or elsewhere.</p>
<p>Although this post isn't on View testing, <a href="http://jakescruggs.blogspot.com/2007/03/mockingstubbing-partials-and-helper.html" target="_blank">this article</a> helps explain how to mocking partials and helpers in views.</p>
