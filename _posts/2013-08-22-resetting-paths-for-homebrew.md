---
layout: post
title: Resetting Paths for Homebrew
date: '2013-08-22 19:31:38 -0700'
categories:
- Mac OS X
tags:
- homebrew
- command line
- cmdline
---
<p>I recently needed to install a program on my Mac using Homebrew. I was instructed to run 'brew update', and then the 'brew doctor' command which resulted in this message:</p>
<pre class="brush:shell">Warning: /usr/bin occurs before /usr/local/bin<br />
This means that system-provided programs will be used instead of those<br />
provided by Homebrew. The following tools exist at both paths:</p>
<p>    gcov-4.2<br />
    git<br />
    git-cvsserver<br />
    git-receive-pack<br />
    git-shell<br />
    git-upload-archive<br />
    git-upload-pack</p>
<p>Consider amending your PATH so that /usr/local/bin<br />
occurs before /usr/bin in your PATH.</pre><br />
I am using Zsh instead of Bash, and checked my .bashrc, .bash_profile, .zshenv, and .zshrc files. None of those expressed the path with the /usr/bin path expressed before the /usr/local/bin.</p>
<p>I also noticed that when using 'echo $PATH' the paths were being duplicated. I saw that in my .zshrc I was setting the path in the correct order, but something else was setting the paths in the incorrect order...and taking precendence.</p>
<p>It turns out that there is a file - /etc/paths - which controls the default paths for all users on the system. I used 'sudo nano /etc/paths' to edit my configuration to reflect the following:</p>
<pre class="brush:shell">/usr/local/bin<br />
/usr/bin<br />
/bin<br />
/usr/sbin<br />
/sbin</pre><br />
I opened a new terminal and ran 'brew doctor' again.</p>
<pre class="brush:shell">$ brew doctor<br />
Your system is ready to brew.</pre><br />
 </p>
<p> </p>
