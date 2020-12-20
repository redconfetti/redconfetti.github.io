---
layout: post
title: MySQL Gem Installation on Mac 10.5.8 - 64 bit??
date: '2010-07-16 17:26:14 -0700'
comments: true
categories:
- Ruby on Rails
---

I'm setting up a new Ruby on Rails application, and tried to run the first
migration for the creation of the new database. This failed because I didn't
have the MySQL gem installed. I'm using a 64 bit processor (Intel Core 2 Duo)
so I installed the 64 bit MySQL for 10.5.8 (Leopard, I haven't upgraded to
Snow Leopard yet).

When trying to run the installation command I received an error:

``` shell
$ sudo gem install mysql
Password:
Building native extensions.  This could take a while...
ERROR:  Error installing mysql:
    ERROR: Failed to build gem native extension.
/opt/local/bin/ruby extconf.rb
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lm... yes
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lz... yes
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lsocket... no
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lnsl... no
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lmygcc... no
checking for mysql_query() in -lmysqlclient... no
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of
necessary libraries and/or headers.  Check the mkmf.log file for more
details.  You may need configuration options.
```

<!--more-->

I looked online for a solution to this, and found that you have to point to the
directory where MySQL is installed. I tried:

``` shell
sudo gem install mysql -- --with-mysql-dir=/usr/local/mysql
```

This resulted in this error instead:

``` shell
Building native extensions.  This could take a while...
ERROR:  Error installing mysql:
    ERROR: Failed to build gem native extension.
/opt/local/bin/ruby extconf.rb --with-mysql-dir=/usr/local/mysql
checking for mysql_ssl_set()... no
checking for rb_str_set_len()... no
checking for rb_thread_start_timer()... no
checking for mysql.h... no
checking for mysql/mysql.h... no
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of
necessary libraries and/or headers.  Check the mkmf.log file for more
details.  You may need configuration options.
```

Since the error is reporting that it can't find mysql.h (header file), I take
it that the MySQL installer didn't include the header files. From the command
line if I go to /usr/local/mysql/include I see the mysql.h right in there.

I removed the preference panel option by opening the System Preferences, then
holding CTRL and clicking on the MySQL option. This gave me an option to click
on to remove it. I then deleted /usr/local/mysql and
/usr/local/mysql-5.1.48-osx10.5-x86_64

``` shell
sudo rm -rf /usr/local/mysql
sudo rm -rf /usr/local/mysql-5.1.48-osx10.5-x86_64/
```

Someone else mentioned something about using the 32-bit version of MySQL, so I
downloaded it and installed it instead (hoping it doesn't conflict with the
processor I'm using). It installed and started up just fine. I ran the command
to install the MySQL Gem again:

``` shell
$ sudo gem install mysql -- --with-mysql-dir=/usr/local/mysql
Building native extensions.  This could take a while...
Successfully installed mysql-2.8.1
1 gem installed
Installing ri documentation for mysql-2.8.1...
No definition for next_result
No definition for field_name
.
.
.
No definition for error_sqlstate
Installing RDoc documentation for mysql-2.8.1...
No definition for next_result
No definition for field_name
.
.
.
No definition for error_sqlstate
```

Finally it installed just fine.
