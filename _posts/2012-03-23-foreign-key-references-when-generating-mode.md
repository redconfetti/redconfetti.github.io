---
layout: post
title: Foreign Key References when Generating Model
date: '2012-03-23 18:15:26 -0700'
categories:
- Model
tags:
- migrations
comments: []
---
<p>I forget the proper syntax for a model generation command that includes a reference to another models id (foreign key).</p>
<p>Here is an example you can use to remember:</p>
<pre class="brush:shell">
rails g model Post user:references title:string body:text<br />
</pre></p>
<p>Since the 'user' model already exists, Rails knows that this should be the user_id field that it generates. I guess it's not a big deal, you could just do 'user_id:integer', but what fun is that?</p>
