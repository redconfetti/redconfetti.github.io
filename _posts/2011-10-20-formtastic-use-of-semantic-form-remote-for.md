---
layout: post
status: publish
published: true
title: Formtastic use of semantic_form_remote_for
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 187
wordpress_url: http://rails.redconfetti.com/?p=187
date: '2011-10-20 15:29:02 -0700'
date_gmt: '2011-10-20 22:29:02 -0700'
categories:
- View
tags: []
comments: []
---
<p>With the update to Formtastic version 2.0.0.rc1 the 'semantic_remote_form_for' method was removed and support was added to 'remote_form_for' used like this:</p>
<pre class="brush:rails"> true, :url => '/contact' do |f| %></pre><br />
This is to conform with the new Rails 3 update where form_remote_for is replaced with form_for with a remote hash value passed to it as an argument.</p>
