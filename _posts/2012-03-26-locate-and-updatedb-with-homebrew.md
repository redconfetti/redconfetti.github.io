---
layout: post
title: Locate and Updatedb with Homebrew
date: '2012-03-26 14:08:59 -0700'
comments: true
categories:
- Mac OS X
tags:
- os x lion
- findutils
- homebrew
---

__UPDATE:__ I ran into errors and decided to not use the findutils provided by
Homebrew.

When you run the `locate` command, the system will now tell you to run the
service that creates the file database:

```shell
WARNING: The locate database (/var/db/locate.database) does not exist.
To create the database, run the following command:

  sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.locate.plist

Please be aware that the database can take some time to generate; once
the database has been created, this message will no longer appear.
```

You're better of simply running this and then using the built in 'locate' command.

----

I used to use MacPorts to ensure that my command line environment on my Mac
was almost exclusively using MacPort provided binaries, not the built in
binaries and libraries that are packaged with Mac OS X.
<!--more-->

I had heard of Homebrew, but MacPorts seemed to work fine for me. Then I
realized that Homebrew does the same thing, but installs software in
/usr/local, which doesn't require sudo. The benefits of Homebrew seem to be
[simplicity, lack of intrusiveness, and speed][1]. I'm likely going to use it
as the package manager in my Rails developer toolkit in the future.

I just noticed that I am not able to use the 'locate' command to search for
certain matching filenames on my system. I love using this option piped into
grep to find what I'm looking for, such as the path to a particular gem I'm
needing to inspect the code for.

I installed the 'findutils' package that includes 'locate' via Homebrew.

```bash
$ brew install findutils

==> Downloading http://ftpmirror.gnu.org/findutils/findutils-4.4.2.tar.gz
######################################################################## 100.0%
==> ./configure --prefix=/usr/local/Cellar/findutils/4.4.2 --program-prefix=g
--disable-debug
==> make install

Warning: Non-libraries were installed to "lib".

Installing non-libraries to "lib" is bad practice.

The offending files are:
/usr/local/Cellar/findutils/4.4.2/lib/charset.alias
==> Summary
/usr/local/Cellar/findutils/4.4.2: 19 files, 1.2M, built in 68 seconds
```

I thought that the warning regarding non-libraries being installed to "lib"
stopped the package from being installed properly, but it turns out that the
executables were installed with symlinks placed in /usr/local/bin (which is in
my path) and pointing to the actual installed binaries. Instead of 'locate'
and 'updatedb', the commands are 'glocate' and 'gupdatedb'.

As advised by [Grogs][2], I updated my .bash_profile file to set the
LOCATE_PATH to point to the database in my local users tmp directory.

I didn't want to have to setup a running cron daemon on my Mac, and I'm just
fine with running the updatedb command manually when needed, so I simply added
aliases to build and locate using the proper executable filenames.

``` shell
alias updatedb="gupdatedb --localpaths='/Users/jmiller' --output='/Users/jmiller/tmp/locatedb'"
alias locate="glocate"
export LOCATE_PATH="~/tmp/locatedb"
```

Before this would work though, I did need to create a 'tmp' folder in my home
directory.

``` shell
mkdir ~/tmp
```

__UPDATE:__

This wasn't successful however. I started to get an error:

```bash
$ updatedb

/usr/bin/sort: string comparison failed: Illegal byte sequence

/usr/bin/sort: Set LC_ALL='C' to work around the problem.

/usr/bin/sort: The strings compared were `/USERS/JMILLER/LIBRARY/APPLICATION SUPPORT/TEXTMATE/BUNDLES/SCSS.TMBUNDLE/COMMANDS/INCREASE NUMBER.TMCOMMAND' and `/USERS/JMILLER/LIBRARY/APPLICATION SUPPORT/TEXTMATE/BUNDLES/SCSS.TMBUNDLE/COMMANDS/INSERT COLOR302200246.TMCOMMAND'.
```

I'm going to just use the locate/updatedb options which come packaged with Mac
OS X.

[1]: http://tedwise.com/2010/08/28/homebrew-vs-macports/
[2]: http://superuser.com/questions/109590/whats-the-equivalent-of-linuxs-updatedb-command-for-the-mac
