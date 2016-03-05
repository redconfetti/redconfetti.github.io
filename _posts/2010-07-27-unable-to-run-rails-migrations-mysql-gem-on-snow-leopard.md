---
layout: post
status: publish
published: true
title: Undefined method 'ref' for ActiveSupport::Dependencies:Module
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 564
wordpress_url: http://www.redconfetti.com/?p=564
date: '2010-07-27 10:19:01 -0700'
date_gmt: '2010-07-27 14:19:01 -0700'
categories:
- Controller
tags: []
comments: []
---
<p>After upgrading to Snow Leopard, and trying to run 'rake db:migrate', I received this error once. This seems common to others which have upgraded, especially back when Snow Leopard was released in August of 2009:</p>
<pre class="brush:bash">
rake aborted!<br />
uninitialized constant MysqlCompat::MysqlRes<br />
(See full trace by running task with --trace)<br />
</pre></p>
<p>I've tried to troubleshoot by reinstalling the MySQL gem, and the 64 bit version of the MySQL server. I'm no longer receiving this error above.</p>
<p>When installing the MySQL Gem, I receive a bunch of errors, unless I specify to not install documentation.</p>
<pre class="brush:bash">
$ sudo env ARCHFLAGS="-arch x86_64" gem install mysql -- --with-mysql-config=/opt/local/lib/mysql5/bin/mysql_config<br />
Building native extensions.  This could take a while...<br />
Successfully installed mysql-2.8.1<br />
1 gem installed<br />
Installing ri documentation for mysql-2.8.1...</p>
<p>No definition for next_result</p>
<p>No definition for field_name</p>
<p>No definition for field_table</p>
<p>No definition for field_def</p>
<p>No definition for field_type</p>
<p>No definition for field_length</p>
<p>No definition for field_max_length</p>
<p>No definition for field_flags</p>
<p>No definition for field_decimals</p>
<p>No definition for time_inspect</p>
<p>No definition for time_to_s</p>
<p>No definition for time_get_year</p>
<p>No definition for time_get_month</p>
<p>No definition for time_get_day</p>
<p>No definition for time_get_hour</p>
<p>No definition for time_get_minute</p>
<p>No definition for time_get_second</p>
<p>No definition for time_get_neg</p>
<p>No definition for time_get_second_part</p>
<p>No definition for time_set_year</p>
<p>No definition for time_set_month</p>
<p>No definition for time_set_day</p>
<p>No definition for time_set_hour</p>
<p>No definition for time_set_minute</p>
<p>No definition for time_set_second</p>
<p>No definition for time_set_neg</p>
<p>No definition for time_set_second_part</p>
<p>No definition for time_equal</p>
<p>No definition for error_errno</p>
<p>No definition for error_sqlstate<br />
Installing RDoc documentation for mysql-2.8.1...</p>
<p>No definition for next_result</p>
<p>No definition for field_name</p>
<p>No definition for field_table</p>
<p>No definition for field_def</p>
<p>No definition for field_type</p>
<p>No definition for field_length</p>
<p>No definition for field_max_length</p>
<p>No definition for field_flags</p>
<p>No definition for field_decimals</p>
<p>No definition for time_inspect</p>
<p>No definition for time_to_s</p>
<p>No definition for time_get_year</p>
<p>No definition for time_get_month</p>
<p>No definition for time_get_day</p>
<p>No definition for time_get_hour</p>
<p>No definition for time_get_minute</p>
<p>No definition for time_get_second</p>
<p>No definition for time_get_neg</p>
<p>No definition for time_get_second_part</p>
<p>No definition for time_set_year</p>
<p>No definition for time_set_month</p>
<p>No definition for time_set_day</p>
<p>No definition for time_set_hour</p>
<p>No definition for time_set_minute</p>
<p>No definition for time_set_second</p>
<p>No definition for time_set_neg</p>
<p>No definition for time_set_second_part</p>
<p>No definition for time_equal</p>
<p>No definition for error_errno</p>
<p>No definition for error_sqlstate<br />
</pre></p>
<p>I've realized that these errors don't occur if you try to install without documentation included. </p>
<pre class="brush:bash">
$ sudo env ARCHFLAGS="-arch x86_64" gem install mysql --no-ri --no-rdoc -- --with-mysql-config=/opt/local/lib/mysql5/bin/mysql_config<br />
Building native extensions.  This could take a while...<br />
Successfully installed mysql-2.8.1<br />
1 gem installed<br />
</pre></p>
<p>I doubted if this really was a successful installation due to the errors when including the documentation, but this isn't the case.</p>
<p>You'll notice that I'm running MySQL from the installation location in /opt/local/lib/mysql5/. This is because I'm using MacPorts, and I definitely do not want to try to run from a mixed environment with some executables and gems being installed under my system folders and library locations, and others under the location of the folders where MacPorts installs these things. Why? Because I read that some people have had problems with 32 bit versions of some libraries and executables still being installed and this causing dependency conflicts. I'm needing certain packages provided by MacPorts, so I'm trying to ensure that my entire environment is based on MacPort packages.</p>
<pre class="brush:bash">
jason-imac:bin jason$ file `which ruby`<br />
/opt/local/bin/ruby: Mach-O 64-bit executable x86_64<br />
jason-imac:bin jason$ file `which mysql`<br />
/opt/local/bin/mysql: Mach-O 64-bit executable x86_64<br />
</pre></p>
<p>I've completely uninstalled all the gems I had installed. I'm only working on one new project, so luckily I have no dependencies on all these old gems. I even <a href="http://blog.costan.us/2009/03/removing-default-ruby-gems-on-osx.html">removed all the gems</a> which came with the system by default. Now all the gems are installed based on the MacPorts version of Ruby I'm running.</p>
<pre class="brush:bash">
$ sudo gem list -d | grep Installed<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
    Installed at: /opt/local/lib/ruby/gems/1.8<br />
</pre></p>
<p>When I run a migration I still received an error. I doubted that the MySQL gem is installed properly, but this I can't be completely sure of. No one else has reported this error, so I'm really stuck.</p>
<pre class="brush:bash">
$ rake db:migrate<br />
(in /Users/jason/rj4)<br />
rake aborted!<br />
undefined method `ref' for ActiveSupport::Dependencies:Module</p>
<p>(See full trace by running task with --trace)<br />
</pre></p>
<p>Finally I realized that this error was the result of some issue with the Devise gem which my co-worker installed for user authentication. I found this <a href="http://groups.google.com/group/plataformatec-devise/browse_thread/thread/b143fe5a08b86ac6">Google Groups</a> posting about the same error.</p>
<p>I installed devise version 1.0.8 and this resolved the issue.</p>
<pre class="brush:bash">
$ sudo gem install devise -v=1.0.8<br />
Successfully installed devise-1.0.8<br />
1 gem installed<br />
Installing ri documentation for devise-1.0.8...<br />
Installing RDoc documentation for devise-1.0.8...</p>
<p>$ rake db:migrate<br />
(in /Users/jason/rj4)<br />
rake aborted!<br />
Unknown database 'rj4_development'</p>
<p>(See full trace by running task with --trace)<br />
jason-imac:rj4 jason$ rake db:create<br />
(in /Users/jason/rj4)<br />
jason-imac:rj4 jason$ rake db:migrate<br />
(in /Users/jason/rj4)<br />
==  CreateProperties: migrating ===============================================<br />
-- create_table(:properties)<br />
   -> 0.0712s<br />
==  CreateProperties: migrated (0.0713s) ======================================</p>
<p>==  CreateUsers: migrating ====================================================<br />
-- create_table(:users)<br />
   -> 0.0967s<br />
==  CreateUsers: migrated (0.0969s) ===========================================<br />
</pre></p>
