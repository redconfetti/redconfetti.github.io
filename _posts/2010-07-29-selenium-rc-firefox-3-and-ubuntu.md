---
layout: post
published: true
title: Selenium RC, Firefox 3, and Ubuntu
date: '2010-07-29 18:46:26 -0700'
date_gmt: '2010-07-29 22:46:26 -0700'
categories:
- web development
tags:
- linkedin
---

![Selenium Logo](/images/posts/seleniumHQ-logo.jpg "Selenium Logo")

I've got a system setup which uses Firefox on an Ubuntu machine, with the Selenium RC server (remote control). I had a set of scripts which would run automatically every 15 minutes, which would prompt Firefox to open and go to the site and submit certain forms. This stopped working after I ran an update on some packages in my Ubuntu machine (9.04 Jaunty).

I was able to resolve this issue by upgrading from [Selenium RC 1.0.1 to 1.0.3](http://seleniumhq.org/download/).
