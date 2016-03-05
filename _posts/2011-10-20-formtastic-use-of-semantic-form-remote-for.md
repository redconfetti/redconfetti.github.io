---
layout: post
title: Formtastic use of semantic_form_remote_for
date: '2011-10-20 15:29:02 -0700'
categories:
- View
tags: []
comments: []
---
With the update to Formtastic version 2.0.0.rc1 the 'semantic_remote_form_for' method was removed and support was added to 'remote_form_for' used like this:

<pre class="brush:rails"> true, :url => '/contact' do |f| %>```

This is to conform with the new Rails 3 update where form_remote_for is replaced with form_for with a remote hash value passed to it as an argument.

