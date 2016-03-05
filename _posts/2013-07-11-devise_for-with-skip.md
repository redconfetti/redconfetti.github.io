---
layout: post
title: Devise_For with Skip
date: '2013-07-11 22:26:51 -0700'
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
