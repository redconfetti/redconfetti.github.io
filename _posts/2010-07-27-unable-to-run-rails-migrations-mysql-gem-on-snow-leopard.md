---
layout: post
title: Undefined method 'ref' for ActiveSupport::Dependencies:Module
date: '2010-07-27 10:19:01 -0700'
categories:
- Controller
tags: []
comments: []
---

After upgrading to Snow Leopard, and trying to run 'rake db:migrate', I received this error once. This seems common to others which have upgraded, especially back when Snow Leopard was released in August of 2009:

``` shell
rake aborted!
uninitialized constant MysqlCompat::MysqlRes
(See full trace by running task with --trace)
```

I've tried to troubleshoot by reinstalling the MySQL gem, and the 64 bit version of the MySQL server. I'm no longer receiving this error above.

When installing the MySQL Gem, I receive a bunch of errors, unless I specify to not install documentation.

``` shell
$ sudo env ARCHFLAGS="-arch x86_64" gem install mysql -- --with-mysql-config=/opt/local/lib/mysql5/bin/mysql_config
Building native extensions.  This could take a while...
Successfully installed mysql-2.8.1
1 gem installed
Installing ri documentation for mysql-2.8.1...
No definition for next_result
No definition for field_name
No definition for field_table
No definition for field_def
No definition for field_type
No definition for field_length
No definition for field_max_length
No definition for field_flags
No definition for field_decimals
No definition for time_inspect
No definition for time_to_s
No definition for time_get_year
No definition for time_get_month
No definition for time_get_day
No definition for time_get_hour
No definition for time_get_minute
No definition for time_get_second
No definition for time_get_neg
No definition for time_get_second_part
No definition for time_set_year
No definition for time_set_month
No definition for time_set_day
No definition for time_set_hour
No definition for time_set_minute
No definition for time_set_second
No definition for time_set_neg
No definition for time_set_second_part
No definition for time_equal
No definition for error_errno
No definition for error_sqlstate
Installing RDoc documentation for mysql-2.8.1...
No definition for next_result
No definition for field_name
No definition for field_table
No definition for field_def
No definition for field_type
No definition for field_length
No definition for field_max_length
No definition for field_flags
No definition for field_decimals
No definition for time_inspect
No definition for time_to_s
No definition for time_get_year
No definition for time_get_month
No definition for time_get_day
No definition for time_get_hour
No definition for time_get_minute
No definition for time_get_second
No definition for time_get_neg
No definition for time_get_second_part
No definition for time_set_year
No definition for time_set_month
No definition for time_set_day
No definition for time_set_hour
No definition for time_set_minute
No definition for time_set_second
No definition for time_set_neg
No definition for time_set_second_part
No definition for time_equal
No definition for error_errno
No definition for error_sqlstate
```

I've realized that these errors don't occur if you try to install without documentation included. 

``` shell

$ sudo env ARCHFLAGS="-arch x86_64" gem install mysql --no-ri --no-rdoc -- --with-mysql-config=/opt/local/lib/mysql5/bin/mysql_config

Building native extensions.  This could take a while...

Successfully installed mysql-2.8.1

1 gem installed

```

I doubted if this really was a successful installation due to the errors when including the documentation, but this isn't the case.

You'll notice that I'm running MySQL from the installation location in /opt/local/lib/mysql5/. This is because I'm using MacPorts, and I definitely do not want to try to run from a mixed environment with some executables and gems being installed under my system folders and library locations, and others under the location of the folders where MacPorts installs these things. Why? Because I read that some people have had problems with 32 bit versions of some libraries and executables still being installed and this causing dependency conflicts. I'm needing certain packages provided by MacPorts, so I'm trying to ensure that my entire environment is based on MacPort packages.

``` shell

jason-imac:bin jason$ file `which ruby`

/opt/local/bin/ruby: Mach-O 64-bit executable x86_64

jason-imac:bin jason$ file `which mysql`

/opt/local/bin/mysql: Mach-O 64-bit executable x86_64

```

I've completely uninstalled all the gems I had installed. I'm only working on one new project, so luckily I have no dependencies on all these old gems. I even <a href="http://blog.costan.us/2009/03/removing-default-ruby-gems-on-osx.html">removed all the gems</a> which came with the system by default. Now all the gems are installed based on the MacPorts version of Ruby I'm running.

``` shell

$ sudo gem list -d | grep Installed
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
    Installed at: /opt/local/lib/ruby/gems/1.8
```

When I run a migration I still received an error. I doubted that the MySQL gem is installed properly, but this I can't be completely sure of. No one else has reported this error, so I'm really stuck.

``` shell
$ rake db:migrate
(in /Users/jason/rj4)
rake aborted!
undefined method `ref' for ActiveSupport::Dependencies:Module
(See full trace by running task with --trace)
```

Finally I realized that this error was the result of some issue with the Devise gem which my co-worker installed for user authentication. I found this <a href="http://groups.google.com/group/plataformatec-devise/browse_thread/thread/b143fe5a08b86ac6">Google Groups</a> posting about the same error.

I installed devise version 1.0.8 and this resolved the issue.

``` shell
$ sudo gem install devise -v=1.0.8
Successfully installed devise-1.0.8
1 gem installed
Installing ri documentation for devise-1.0.8...
Installing RDoc documentation for devise-1.0.8...
$ rake db:migrate
(in /Users/jason/rj4)
rake aborted!
Unknown database 'rj4_development'
(See full trace by running task with --trace)
$ rake db:create
(in /Users/jason/rj4)
$ rake db:migrate
(in /Users/jason/rj4)
==  CreateProperties: migrating ===============================================
-- create_table(:properties)
   -> 0.0712s
==  CreateProperties: migrated (0.0713s) ======================================
==  CreateUsers: migrating ====================================================
-- create_table(:users)
   -> 0.0967s
==  CreateUsers: migrated (0.0969s) ===========================================
```

