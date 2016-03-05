---
layout: post
status: publish
published: true
title: 'Error: ''unintitialized constant MySQL'' with Rails 3 on Snow Leopard Mac'
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 788
wordpress_url: http://www.redconfetti.com/?p=788
date: '2011-05-19 23:05:08 -0700'
date_gmt: '2011-05-20 03:05:08 -0700'
categories:
- Ruby on Rails
tags: []
comments: []
---
<p>I just installed Rails 3 on my iMac, which is running Snow Leopard. I'm trying to build a web hosting website/billing system/management system. I configured the app to use MySQL in /config/database.yml like so</p>
<pre class="brush:rails">
development:<br />
  adapter: mysql<br />
  encoding: utf8<br />
  database: hosting_development<br />
  username: root<br />
  password:<br />
  host: 127.0.0.1<br />
</pre></p>
<p>I had to do this because I created the Rails app without specifying that I didn't  want sqlite3. I then ran rake db:create and I got this error:</p>
<pre class="brush:bash">
rake aborted!<br />
uninitialized constant Mysql<br />
</pre></p>
<p>I found <a href="http://bparanj.blogspot.com/2010/07/uninitialized-constant-mysql-while.html" target="_blank">an article</a> online which advised to do the follow, with the exception of the path to mysql_config. I'm using MacPorts for most of the packages in my development environment.</p>
<pre class="brush:bash">
$ sudo gem uninstall mysql<br />
$ sudo env ARCHFLAGS="-arch x86_64" gem install mysql -- --with-mysql-config=/opt/local/lib/mysql5/bin/mysql_config<br />
</pre></p>
<p>I then modified the Gemfile in my app with the following. I think this might have been what was needed most, not the gem reinstallation. I forgot to do this until right before it was fixed.</p>
<pre class="brush:rails">
# gem 'sqlite3'<br />
gem 'mysql'<br />
</pre></p>
