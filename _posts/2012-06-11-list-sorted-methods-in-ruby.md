---
layout: post
status: publish
published: true
title: List Sorted Methods in Ruby
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1269
wordpress_url: http://www.rubycoloredglasses.com/?p=1269
date: '2012-06-11 18:06:48 -0700'
date_gmt: '2012-06-11 18:06:48 -0700'
categories:
- Ruby
tags: []
comments: []
---
<p>I often use 'methods' to get a list of methods available for an object in Ruby, but it can be a pain trying to look through the list for what I want. I wish it outputed in a sorted list straight down the page. This template will help you achieve that. Maybe I should override the 'methods' method. Hm...</p>
<pre class="brush:rails">
"object".methods.sort.each do |method| puts method end<br />
</pre></p>
<p>If you want to get only methods with a certain string inside them, use this:</p>
<pre class="brush:rails">
"object".methods.sort.each do |method| puts method if method.to_s.index('search_string') end<br />
</pre></p>
