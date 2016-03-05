---
layout: post
title: Sharing Administrative Rights with Homebrew
date: '2014-06-29 18:52:42 -0700'
categories:
- Mac OS X
tags:
- homebrew
- mac-osx
- permissions
---
<p>I installed <a href="http://brew.sh/" target="_blank">Homebrew</a> on my work computer, and have installed many ports using Homebrew from an account on my machine. This has resulted in all of the files and folders managed by Homebrew being owned by the user account I installed the ports from, with 'admin' group ownership.</p>
<p>Recently I created another account on my machine, logged into it, and ran 'brew doctor' just to make sure everything was in excellent order, and I ran into these errors:</p>
<pre class="brush:bash">$ brew doctor<br />
Warning: /usr/local/etc isn't writable.<br />
This can happen if you "sudo make install" software that isn't managed by<br />
by Homebrew. If a brew tries to write a file to this directory, the<br />
install will fail during the link step.</p>
<p>You should probably `chown` /usr/local/etc</p>
<p>Warning: /usr/local/include isn't writable.<br />
This can happen if you "sudo make install" software that isn't managed by<br />
by Homebrew. If a brew tries to write a file to this directory, the<br />
install will fail during the link step.</p>
<p>You should probably `chown` /usr/local/include</p>
<p>Warning: /usr/local/lib isn't writable.<br />
This can happen if you "sudo make install" software that isn't managed by<br />
by Homebrew. If a brew tries to write a file to this directory, the<br />
install will fail during the link step.</p>
<p>You should probably `chown` /usr/local/lib</p>
<p>Warning: /usr/local/lib/pkgconfig isn't writable.<br />
This can happen if you "sudo make install" software that isn't managed by<br />
by Homebrew. If a brew tries to write a file to this directory, the<br />
install will fail during the link step.</p>
<p>You should probably `chown` /usr/local/lib/pkgconfig</p>
<p>Warning: /usr/local/share isn't writable.<br />
This can happen if you "sudo make install" software that isn't managed by<br />
by Homebrew. If a brew tries to write a file to this directory, the<br />
install will fail during the link step.</p>
<p>You should probably `chown` /usr/local/share</p>
<p>Warning: Some directories in /usr/local/share/man aren't writable.<br />
This can happen if you "sudo make install" software that isn't managed<br />
by Homebrew. If a brew tries to add locale information to one of these<br />
directories, then the install will fail during the link step.<br />
You should probably `chown` them:</p>
<p>    /usr/local/share/man<br />
    /usr/local/share/man/man1<br />
    /usr/local/share/man/man3<br />
    /usr/local/share/man/man5<br />
    /usr/local/share/man/man7<br />
    /usr/local/share/man/man8<br />
</pre><br />
It became clear that this is happening because these files and folders are owned by my other user account. I see that they're associated with the 'admin' group, so I just figured that I needed to add my new account to the 'admin' group. I did this using the following command:</p>
<pre class="brush:bash">sudo dseditgroup -o edit -a usernametoadd -t user admin<br />
</pre><br />
Still this command did not resolve my issue. Further investigation showed that my account was created as an Administrator, so I should be in this 'admin' group already.</p>
<p>I found many articles online that suggested other various things, including adding a 'brew' group and changing all the files to be owned by this group. I don't recommend this, because Homebrew is already using an appropriate group, 'admin', as the default. Homebrew updated the package manager to <a href="https://github.com/Homebrew/homebrew/issues/7308" target="_blank">use the 'admin' group</a> for all files/folders setup under /usr/local back in 2011.</p>
<p>I looked at the group permissions of the files/folders and noticed that the group did not have write permission. I used the following commands, taken from a <a href="http://apple.stackexchange.com/questions/42127/homebrew-permissions-multiple-users-needing-to-brew-update" target="_blank">StackExchange post</a>, which resolved my issue.</p>
<pre class="brush:bash">chmod -R g+w /usr/local<br />
chmod -R g+w /Library/Caches/Homebrew<br />
</pre></p>
<h3>Update 1</h3><br />
This morning I went back to work and found that my Postgres server was not running. I checked the logs and found this error:</p>
<pre class="brush:bash">$ tail -F /usr/local/var/postgres/server.log<br />
LOG:  shutting down<br />
LOG:  database system is shut down<br />
FATAL:  data directory "/usr/local/var/postgres" has group or world access<br />
DETAIL:  Permissions should be u=rwx (0700).<br />
FATAL:  data directory "/usr/local/var/postgres" has group or world access<br />
DETAIL:  Permissions should be u=rwx (0700).<br />
FATAL:  data directory "/usr/local/var/postgres" has group or world access<br />
DETAIL:  Permissions should be u=rwx (0700).<br />
FATAL:  data directory "/usr/local/var/postgres" has group or world access<br />
DETAIL:  Permissions should be u=rwx (0700).<br />
</pre><br />
This resulted from the permissions change I made. Changing the permissions as advised resolved the issue.</p>
<pre class="brush:bash">$ chmod 700 /usr/local/var/postgres<br />
$ ls -la /usr/local/var/<br />
total 0<br />
drwx-w----   9 johnsmith   admin  306 Jun 20 15:31 .<br />
drwxrwxr-x  19 root        admin  646 Jun  2 11:15 ..<br />
drwxrwxr-x   3 johnsmith   admin  102 Oct  8  2013 db<br />
drwxrwxr-x   4 redconfetti admin  136 Jun 20 15:31 log<br />
drwxrwxr-x   6 johnsmith   admin  204 Jun 20 15:47 mongodb<br />
drwxrwxr-x   8 johnsmith   staff  272 Oct  9  2013 mysql<br />
drwx------  21 johnsmith   admin  714 Jun 27 17:45 postgres<br />
drwxrwxr-x   2 johnsmith   admin   68 Oct  8  2013 run<br />
-rw-rw-r--   1 johnsmith   admin    0 Feb 25 16:45 stdout<br />
</pre></p>
<h3>Update 2</h3><br />
It appears that these instructions also caused issues with the permissions of the plist files, which store the configuration of the services launched by launchd. The plist files for Memcached and Redis are symlinked in /Users/myuser/Library/LaunchAgents/ to their location to files like /usr/local/opt/service-name/homebrew.mxcl.service-name.plist. If these have the wrong permissions, launchd will not use those configurations to launch the service.</p>
<pre class="brush:bash">
lunchy start redis<br />
launchctl: Dubious permissions on file (skipping): /Users/johnsmith/Library/LaunchAgents/homebrew.mxcl.redis.plist<br />
nothing found to load<br />
started homebrew.mxcl.redis<br />
</pre></p>
<p>Even though it said that Redis was started, it was not actually started. Here is the command I ran to resolve this:</p>
<pre class="brush:bash">
$ sudo chmod 644 /Users/johnsmith/Library/LaunchAgents/homebrew.mxcl.memcached.plist<br />
$ sudo chmod 644 /Users/johnsmith/Library/LaunchAgents/homebrew.mxcl.redis.plist<br />
</pre></p>
<p>I'm starting to think that there isn't an ideal solution to this issue. Now my plist files cannot be updated by Homebrew.</p>
