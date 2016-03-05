---
layout: post
status: publish
published: true
title: Listing Gems from Rails Console
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1285
wordpress_url: http://www.rubycoloredglasses.com/?p=1285
date: '2012-06-20 21:59:03 -0700'
date_gmt: '2012-06-20 21:59:03 -0700'
categories:
- Uncategorized
tags: []
comments: []
---
<p>Got this from <a href="http://stackoverflow.com/questions/2747990/is-there-any-way-to-tell-which-gems-and-plugins-are-loaded-at-runtime-for-a-rail">Stack Overflow</a>, figured it could come in handy at some point in the future.</p>
<pre class="brush:ruby">Gem.loaded_specs.values.map {|x| "#{x.name} #{x.version}"}</pre></p>
