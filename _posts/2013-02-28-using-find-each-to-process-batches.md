---
layout: post
status: publish
published: true
title: Using Find Each to Process Batches
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1469
wordpress_url: http://www.rubycoloredglasses.com/?p=1469
date: '2013-02-28 20:49:14 -0800'
date_gmt: '2013-02-28 20:49:14 -0800'
categories:
- Ruby on Rails
tags:
- batch processing
comments: []
---
<p>I just found out that there is a <a href="http://apidock.com/rails/ActiveRecord/Batches/ClassMethods/find_each" target="_blank">find_each</a> method provided by ActiveRecord which loops through an array of models that are retrieved in batches of 1000 at a time.</p>
<p>The find is performed by find_in_batches with a batch size of 1000 (or as specified by the :batch_size option).</p>
<pre class="brush:ruby">User.find_each(:start => 2000, :batch_size => 5000) do |user|<br />
  user.do_something<br />
end</pre></p>