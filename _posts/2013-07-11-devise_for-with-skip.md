---
layout: post
status: publish
published: true
title: Devise_For with Skip
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1589
wordpress_url: http://www.rubycoloredglasses.com/?p=1589
date: '2013-07-11 22:26:51 -0700'
date_gmt: '2013-07-11 22:26:51 -0700'
categories:
- Ruby on Rails
tags:
- devise
comments: []
---
<p>I just stumbled upon the options for devise_for which let you auto-generate the routes that are needed for a certain devise resource (user), with certain categories of routes skipped.</p>
<p>For example, if I want to define routes for my User, I can define</p>
<pre class="brush:ruby">devise_for :users</pre><br />
This results in the routes for these three categories:</p>
<ul>
<li><span style="line-height: 12px;">Sessions - Sign in, Sign out</span></li>
<li>Passwords - Password reset options</li>
<li>Registrations - Creating new user, updating existing user, or destroying your user account</li><br />
</ul><br />
You can leave one of these categories out of the route definition by using skip. For instance if you want only the Sign-in and Sign-out options, you could define this in your routes.rb file:</p>
<pre class="brush:ruby">devise_for :users, :skip => [:registrations, :passwords]</pre><br />
 </p>
<p> </p>
