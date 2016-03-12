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
I recently needed to install a program on my Mac using Homebrew. I was instructed to run 'brew update', and then the 'brew doctor' command which resulted in this message:

``` shell
Warning: /usr/bin occurs before /usr/local/bin
This means that system-provided programs will be used instead of those
provided by Homebrew. The following tools exist at both paths:
    gcov-4.2
    git
    git-cvsserver
    git-receive-pack
    git-shell
    git-upload-archive
    git-upload-pack
Consider amending your PATH so that /usr/local/bin
occurs before /usr/bin in your PATH.
```

I am using Zsh instead of Bash, and checked my .bashrc, .bash_profile, .zshenv, and .zshrc files. None of those expressed the path with the /usr/bin path expressed before the /usr/local/bin.

I also noticed that when using 'echo $PATH' the paths were being duplicated. I saw that in my .zshrc I was setting the path in the correct order, but something else was setting the paths in the incorrect order...and taking precendence.

It turns out that there is a file - /etc/paths - which controls the default paths for all users on the system. I used 'sudo nano /etc/paths' to edit my configuration to reflect the following:

``` shell
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
```

I opened a new terminal and ran 'brew doctor' again.

``` shell
$ brew doctor
Your system is ready to brew.
```
