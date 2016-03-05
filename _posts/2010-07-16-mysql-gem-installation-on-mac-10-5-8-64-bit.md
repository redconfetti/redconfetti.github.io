---
layout: post
title: MySQL Gem Installation on Mac 10.5.8 - 64 bit ??
date: '2010-07-16 17:26:14 -0700'
categories:
- Ruby on Rails
tags: []
comments: []
---
<p>I'm setting up a new Ruby on Rails application, and tried to run the first migration for the creation of the new database. This failed because I didn't have the MySQL gem installed. I'm using a 64 bit processor (Intel Core 2 Duo) so I installed the 64 bit MySQL for 10.5.8 (Leopard, I haven't upgraded to Snow Leopard yet).</p>
<p>When trying to run the installation command I received an error:</p>
<pre class="brush:bash">
$ sudo gem install mysql<br />
Password:</p>
<p>Building native extensions.  This could take a while...<br />
ERROR:  Error installing mysql:<br />
    ERROR: Failed to build gem native extension.</p>
<p>/opt/local/bin/ruby extconf.rb<br />
checking for mysql_query() in -lmysqlclient... no<br />
checking for main() in -lm... yes<br />
checking for mysql_query() in -lmysqlclient... no<br />
checking for main() in -lz... yes<br />
checking for mysql_query() in -lmysqlclient... no<br />
checking for main() in -lsocket... no<br />
checking for mysql_query() in -lmysqlclient... no<br />
checking for main() in -lnsl... no<br />
checking for mysql_query() in -lmysqlclient... no<br />
checking for main() in -lmygcc... no<br />
checking for mysql_query() in -lmysqlclient... no<br />
*** extconf.rb failed ***<br />
Could not create Makefile due to some reason, probably lack of<br />
necessary libraries and/or headers.  Check the mkmf.log file for more<br />
details.  You may need configuration options.<br />
</pre></p>
<p>I looked online for a solution to this, and found that you have to point to the directory where MySQL is installed. I tried:</p>
<pre class="brush:bash">
sudo gem install mysql -- --with-mysql-dir=/usr/local/mysql<br />
</pre></p>
<p>This resulted in this error instead:</p>
<pre class="brush:bash">
Building native extensions.  This could take a while...<br />
ERROR:  Error installing mysql:<br />
    ERROR: Failed to build gem native extension.</p>
<p>/opt/local/bin/ruby extconf.rb --with-mysql-dir=/usr/local/mysql<br />
checking for mysql_ssl_set()... no<br />
checking for rb_str_set_len()... no<br />
checking for rb_thread_start_timer()... no<br />
checking for mysql.h... no<br />
checking for mysql/mysql.h... no<br />
*** extconf.rb failed ***<br />
Could not create Makefile due to some reason, probably lack of<br />
necessary libraries and/or headers.  Check the mkmf.log file for more<br />
details.  You may need configuration options.<br />
</pre></p>
<p>Since the error is reporting that it can't find mysql.h (header file), I take it that the MySQL installer didn't include the header files. From the command line if I go to /usr/local/mysql/include I see the mysql.h right in there.</p>
<p>I removed the preference panel option by opening the System Preferences, then holding CTRL and clicking on the MySQL option. This gave me an option to click on to remove it. I then deleted /usr/local/mysql and /usr/local/mysql-5.1.48-osx10.5-x86_64</p>
<pre class="brush:bash">
sudo rm -rf /usr/local/mysql<br />
sudo rm -rf /usr/local/mysql-5.1.48-osx10.5-x86_64/<br />
</pre></p>
<p>Someone else mentioned something about using the 32-bit version of MySQL, so I downloaded it and installed it instead (hoping it doesn't conflict with the processor I'm using). It installed and started up just fine. I ran the command to install the MySQL Gem again:</p>
<pre class="brush:bash">
$ sudo gem install mysql -- --with-mysql-dir=/usr/local/mysql<br />
Building native extensions.  This could take a while...<br />
Successfully installed mysql-2.8.1<br />
1 gem installed<br />
Installing ri documentation for mysql-2.8.1...</p>
<p>No definition for next_result</p>
<p>No definition for field_name</p>
<p>.<br />
.<br />
.<br />
No definition for error_sqlstate<br />
Installing RDoc documentation for mysql-2.8.1...</p>
<p>No definition for next_result</p>
<p>No definition for field_name<br />
.<br />
.<br />
.<br />
No definition for error_sqlstate<br />
</pre></p>
<p>Finally it installed just fine.</p>
