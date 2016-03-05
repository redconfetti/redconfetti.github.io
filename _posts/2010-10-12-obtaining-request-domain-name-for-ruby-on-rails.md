---
layout: post
status: publish
published: true
title: Obtaining Request Domain Name for Ruby on Rails
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 640
wordpress_url: http://www.redconfetti.com/?p=640
date: '2010-10-12 01:19:16 -0700'
date_gmt: '2010-10-12 05:19:16 -0700'
categories:
- Ruby on Rails
tags: []
comments: []
---
<p>I'm using Rails 2.3.8. To obtain the domain name for the website being requested (i.e. mysite.com, mysite.net), just reference 'request.host'.</p>
<pre class="brush:rails">@host = request.host</pre></p>
<p>You can only reference request.host in the views, or the controller.</p>
