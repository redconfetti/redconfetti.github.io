---
layout: post
title: Using Find Each to Process Batches
date: '2013-02-28 20:49:14 -0800'
categories:
- Ruby on Rails
tags:
- batch processing
comments: []
---
I just found out that there is a <a href="http://apidock.com/rails/ActiveRecord/Batches/ClassMethods/find_each" target="_blank">find_each</a> method provided by ActiveRecord which loops through an array of models that are retrieved in batches of 1000 at a time.

The find is performed by find_in_batches with a batch size of 1000 (or as specified by the :batch_size option).

<pre class="brush:ruby">User.find_each(:start => 2000, :batch_size => 5000) do |user|

  user.do_something

end```

