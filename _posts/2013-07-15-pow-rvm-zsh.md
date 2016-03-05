---
layout: post
status: publish
published: true
title: POW RVM ZSH
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1592
wordpress_url: http://www.rubycoloredglasses.com/?p=1592
date: '2013-07-15 00:07:38 -0700'
date_gmt: '2013-07-15 00:07:38 -0700'
categories:
- Ruby on Rails
tags:
- rvm
- pow
- zsh
comments: []
---
<p>I'm using a Rails 3 app, and my colleague updated the RVM config to use Ruby 2.0.0. I was having issues getting POW to work with the app. I'm using ZSH as my shell also.</p>
<p>The following command resolved my issue.</p>
<pre class="brush:shell">rvm env . -- --env > .powenv</pre><br />
Props to <a href="http://stackoverflow.com/questions/10154928/pow-rvm-and-zsh-not-working-together" target="_blank">Linus on StackOverflow</a> for this solution.</p>
<p> </p>
