---
layout: post
title: Obtaining Request Domain Name for Ruby on Rails
date: '2010-10-12 01:19:16 -0700'
categories:
- Ruby on Rails
tags: []
comments: []
---
<p>I'm using Rails 2.3.8. To obtain the domain name for the website being requested (i.e. mysite.com, mysite.net), just reference 'request.host'.</p>
<pre class="brush:rails">@host = request.host</pre></p>
<p>You can only reference request.host in the views, or the controller.</p>
