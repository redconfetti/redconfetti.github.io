---
layout: post
title: Sharing Administrative Rights with Homebrew
date: '2014-06-29 18:52:42 -0700'
comments: true
categories:
- Mac OS X
tags:
- homebrew
- mac-osx
- permissions
---

I installed [Homebrew](http://brew.sh/) on my work computer, and have installed
many ports using Homebrew from an account on my machine. This has resulted in
all of the files and folders managed by Homebrew being owned by the user account
I installed the ports from, with 'admin' group ownership.

Recently I created another account on my machine, logged into it, and ran
'brew doctor' just to make sure everything was in excellent order, and I ran
into these errors:
<!--more-->

``` shell
$ brew doctor

Warning: /usr/local/etc isn't writable.
This can happen if you "sudo make install" software that isn't managed by
by Homebrew. If a brew tries to write a file to this directory, the
install will fail during the link step.
You should probably `chown` /usr/local/etc
Warning: /usr/local/include isn't writable.

This can happen if you "sudo make install" software that isn't managed by
by Homebrew. If a brew tries to write a file to this directory, the
install will fail during the link step.

You should probably `chown` /usr/local/include
Warning: /usr/local/lib isn't writable.
This can happen if you "sudo make install" software that isn't managed by
by Homebrew. If a brew tries to write a file to this directory, the install will fail during the link step.

You should probably `chown` /usr/local/lib
Warning: /usr/local/lib/pkgconfig isn't writable.

This can happen if you "sudo make install" software that isn't managed by by Homebrew. If a brew tries to write a file to this directory, the install will fail during the link step.

You should probably `chown` /usr/local/lib/pkgconfig
Warning: /usr/local/share isn't writable.
This can happen if you "sudo make install" software that isn't managed by by Homebrew. If a brew tries to write a file to this directory, the install will fail during the link step.

You should probably `chown` /usr/local/share
Warning: Some directories in /usr/local/share/man aren't writable.
This can happen if you "sudo make install" software that isn't managed by Homebrew. If a brew tries to add locale information to one of these directories, then the install will fail during the link step.

You should probably `chown` them:
    /usr/local/share/man
    /usr/local/share/man/man1
    /usr/local/share/man/man3
    /usr/local/share/man/man5
    /usr/local/share/man/man7
    /usr/local/share/man/man8
```

It became clear that this is happening because these files and folders are owned
by my other user account. I see that they're associated with the 'admin' group,
so I just figured that I needed to add my new account to the 'admin' group. I
did this using the following command:

``` shell
sudo dseditgroup -o edit -a usernametoadd -t user admin
```

Still this command did not resolve my issue. Further investigation showed that
my account was created as an Administrator, so I should be in this 'admin'
group already.

I found many articles online that suggested other various things, including
adding a 'brew' group and changing all the files to be owned by this group. I
don't recommend this, because Homebrew is already using an appropriate group,
'admin', as the default. Homebrew updated the package manager to
[use the 'admin' group](https://github.com/Homebrew/homebrew/issues/7308) for
all files/folders setup under /usr/local back in 2011.

I looked at the group permissions of the files/folders and noticed that the
group did not have write permission. I used the following commands, taken from
a [StackExchange post], which resolved my issue.

``` shell
chmod -R g+w /usr/local
chmod -R g+w /Library/Caches/Homebrew
```

[StackExchange post]: http://apple.stackexchange.com/questions/42127/homebrew-permissions-multiple-users-needing-to-brew-update

### Update 1

This morning I went back to work and found that my Postgres server was not
running. I checked the logs and found this error:

``` shell
$ tail -F /usr/local/var/postgres/server.log
LOG:  shutting down
LOG:  database system is shut down
FATAL:  data directory "/usr/local/var/postgres" has group or world access
DETAIL:  Permissions should be u=rwx (0700).
FATAL:  data directory "/usr/local/var/postgres" has group or world access
DETAIL:  Permissions should be u=rwx (0700).
FATAL:  data directory "/usr/local/var/postgres" has group or world access
DETAIL:  Permissions should be u=rwx (0700).
FATAL:  data directory "/usr/local/var/postgres" has group or world access
DETAIL:  Permissions should be u=rwx (0700).
```

This resulted from the permissions change I made. Changing the permissions as
advised resolved the issue.

``` shell
$ chmod 700 /usr/local/var/postgres
$ ls -la /usr/local/var/
total 0
drwx-w----   9 johnsmith   admin  306 Jun 20 15:31 .
drwxrwxr-x  19 root        admin  646 Jun  2 11:15 ..
drwxrwxr-x   3 johnsmith   admin  102 Oct  8  2013 db
drwxrwxr-x   4 redconfetti admin  136 Jun 20 15:31 log
drwxrwxr-x   6 johnsmith   admin  204 Jun 20 15:47 mongodb
drwxrwxr-x   8 johnsmith   staff  272 Oct  9  2013 mysql
drwx------  21 johnsmith   admin  714 Jun 27 17:45 postgres
drwxrwxr-x   2 johnsmith   admin   68 Oct  8  2013 run
-rw-rw-r--   1 johnsmith   admin    0 Feb 25 16:45 stdout
```

### Update 2

It appears that these instructions also caused issues with the permissions of
the plist files, which store the configuration of the services launched by
launchd. The plist files for Memcached and Redis are symlinked in
`/Users/myuser/Library/LaunchAgents/` to their location to files like
`/usr/local/opt/service-name/homebrew.mxcl.service-name.plist`. If these have
the wrong permissions, launchd will not use those configurations to launch the
service.

``` shell
lunchy start redis
launchctl: Dubious permissions on file (skipping): /Users/johnsmith/Library/LaunchAgents/homebrew.mxcl.redis.plist
nothing found to load
started homebrew.mxcl.redis
```

Even though it said that Redis was started, it was not actually started. Here
is the command I ran to resolve this:

``` shell
sudo chmod 644 /Users/johnsmith/Library/LaunchAgents/homebrew.mxcl.memcached.plist
sudo chmod 644 /Users/johnsmith/Library/LaunchAgents/homebrew.mxcl.redis.plist
```

I'm starting to think that there isn't an ideal solution to this issue. Now my
plist files cannot be updated by Homebrew.
