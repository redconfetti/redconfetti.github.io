---
layout: post
status: publish
published: true
title: Selenium - no display specified
author: maxwell keyes
date: '2010-01-11 14:51:10 -0800'
date_gmt: '2010-01-11 18:51:10 -0800'
categories:
- computing tips
tags: []
---

I'm not very experienced with X-windows on the Linux platform, so I'm not too
skilled in troubleshooting issues with the display. I recently upgraded an
Ubuntu system at work to use Ubuntu 9.04 (Jaunty Jackalope), which only has
Firefox 3 available (no package for Firefox 2). I had a Selenium server setup
running tests, but they stopped working after I upgraded to this newer version
of Ubuntu.

I thought that perhaps Selenium wasn't compatible with version 3 of Firefox,
but this isn't the case. The Selenium website says ['Firefox 2+' for browsers
running on Linux][Selenium OS Support].
<!--more-->

The error I was receiving when I would run a test was:

```bash
10:46:19.778 INFO - Preparing Firefox profile...
Error: no display specified
```

After a bunch of research and Googling online, it turned out I just needed to
run this before I started my Selenium server:

```bash
export DISPLAY=:0
```

I hope this saves someone else some time.

[Selenium OS Support]: http://seleniumhq.org/about/platforms.html#operating-systems
