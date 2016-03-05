---
layout: post
title: Listing Gems from Rails Console
date: '2012-06-20 21:59:03 -0700'
categories:
- Uncategorized
tags: []
comments: []
---
<p>Got this from <a href="http://stackoverflow.com/questions/2747990/is-there-any-way-to-tell-which-gems-and-plugins-are-loaded-at-runtime-for-a-rail">Stack Overflow</a>, figured it could come in handy at some point in the future.</p>
<pre class="brush:ruby">Gem.loaded_specs.values.map {|x| "#{x.name} #{x.version}"}</pre></p>
