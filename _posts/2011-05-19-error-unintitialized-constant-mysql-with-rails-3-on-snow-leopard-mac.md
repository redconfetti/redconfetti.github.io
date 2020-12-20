---
layout: post
title: 'Error: ''unintitialized constant MySQL'' with Rails 3 on Snow Leopard Mac'
date: '2011-05-19 23:05:08 -0700'
comments: true
categories:
- Ruby on Rails
---

I just installed Rails 3 on my iMac, which is running Snow Leopard. I'm trying
to build a web hosting website/billing system/management system. I configured
the app to use MySQL in /config/database.yml like so:

``` ruby
development:
  adapter: mysql
  encoding: utf8
  database: hosting_development
  username: root
  password:
  host: 127.0.0.1
```

I had to do this because I created the Rails app without specifying that I
didn't  want sqlite3. I then ran rake db:create and I got this error:
<!--more-->

``` shell
rake aborted!
uninitialized constant Mysql
```

I found [an article][1] online which advised to do the follow, with the
exception of the path to mysql_config. I'm using MacPorts for most of the
packages in my development environment.

``` shell
$ sudo gem uninstall mysql
$ sudo env ARCHFLAGS="-arch x86_64" gem install mysql --
--with-mysql-config=/opt/local/lib/mysql5/bin/mysql_config
```

I then modified the Gemfile in my app with the following. I think this might
have been what was needed most, not the gem reinstallation. I forgot to do
this until right before it was fixed.

``` ruby
# gem 'sqlite3'
gem 'mysql'
```

[1]: http://bparanj.blogspot.com/2010/07/uninitialized-constant-mysql-while.html
