---
layout: post
status: publish
published: true
title: Changing Column Order via ActiveRecord Migration
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 605
wordpress_url: http://www.redconfetti.com/?p=605
date: '2010-08-15 22:09:40 -0700'
date_gmt: '2010-08-16 02:09:40 -0700'
categories:
- Ruby on Rails
tags: []
comments: []
---
<p>Is it possible to change the order of the columns in your MySQL (or other database) table using a migration? Lets see.</p>
<p>If you check the <a href="http://api.rubyonrails.org/classes/ActiveRecord/Migration.html">ActiveRecord::Migration documentation</a> you'll see there is a method called 'change_column' which accepts various options.</p>
<pre>
<tt>change_column(table_name, column_name, type, options)</tt>:<br />
Changes the column to a different type using the same parameters as add_column<br />
</pre><br />
As of Rails 2.3.6 this is now available by using the :after option. You'll have to include the field type, even though you are not modifying the type.</p>
<p>Example:</p>
<pre class="brush:rails">
change_column :orders, :tax_rate, :float, :after => :tax_state<br />
</pre></p>