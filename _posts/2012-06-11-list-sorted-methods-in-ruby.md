---
layout: post
title: List Sorted Methods in Ruby
date: '2012-06-11 18:06:48 -0700'
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
