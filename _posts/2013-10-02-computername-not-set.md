---
layout: post
title: 'ComputerName: not set'
date: '2013-10-02 23:20:22 -0700'
categories:
- Mac OS X
tags:
- oh-my-zsh
---
<p>I recently installed <a href="https://github.com/robbyrussell/oh-my-zsh" target="_blank">Oh-my-Zsh</a> on a new Macbook Pro running Mountain Lion. When I opened up my terminal, I received the message "ComputerName: not set".</p>
<p>I tried to use the 'sudo hostname' command, but this didn't seem to work. I ended up opening System Preferences -> Sharing, and then set my Computer Name.</p>
<p>I found <a href="http://apple.stackexchange.com/questions/66611/how-to-change-computer-name-so-terminal-displays-it-in-mac-os-x-mountain-lion" target="_blank">an article</a> that also suggested using the following:</p>
<pre class="brush:shell">sudo scutil --set ComputerName "newname"<br />
sudo scutil --set LocalHostName "newname"<br />
sudo scutil --set HostName "newname"</pre></p>
