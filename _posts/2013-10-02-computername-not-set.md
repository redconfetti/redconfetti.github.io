---
layout: post
title: 'ComputerName: not set'
date: '2013-10-02 23:20:22 -0700'
categories:
- Mac OS X
tags:
- oh-my-zsh
---
I recently installed [Oh-my-Zsh] on a new Macbook Pro running Mountain Lion.
When I opened up my terminal, I received the message "ComputerName: not set".

I tried to use the 'sudo hostname' command, but this didn't seem to work. I
ended up opening System Preferences -> Sharing, and then set my Computer Name.
<!--more-->

I found [an article] that also suggested using the following:

``` shell
sudo scutil --set ComputerName "newname"
sudo scutil --set LocalHostName "newname"
sudo scutil --set HostName "newname"
```

[Oh-my-Zsh]: https://github.com/robbyrussell/oh-my-zsh
[an article]: http://apple.stackexchange.com/questions/66611/how-to-change-computer-name-so-terminal-displays-it-in-mac-os-x-mountain-lion
