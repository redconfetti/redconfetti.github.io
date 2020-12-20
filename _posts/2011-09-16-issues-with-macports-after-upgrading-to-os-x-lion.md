---
layout: post
title: Issues with MacPorts After Upgrading to OS X Lion
date: '2011-09-16 18:10:00 -0700'
comments: true
categories:
- Mac OS X
tags:
- macports
---

I realized this morning that I was having dependency issues with ImageMagick
on my Mac, which I installed using [MacPorts][1]. I had recently upgraded to
Mac OS X Lion, so it made sense that I needed to update the software to
resolve the issues, much like I had when I upgraded to Snow Leopard.

I found [this article][2] that provided steps for migrating MacPorts for Lion,
but I kept getting this error when I tried to uninstall all the packages:

```shell
warning: Failed to execute portfile from registry for apache2
@2.2.17_1+preforkmpm too many nested evaluations (infinite loop?)
Warning: Failed to execute portfile from registry for apache2
@2.2.17_1+preforkmpm too many nested evaluations (infinite loop?)
Warning: Failed to execute portfile from registry for apache2
@2.2.17_1+preforkmpm too many nested evaluations (infinite loop?)
Warning: Failed to execute portfile from registry for apache2
@2.2.17_1+preforkmpm too many nested evaluations (infinite loop?)
```

<!--more-->

I searched and searched for a solution, and even tried to uninstall apache2
@2.2.17_1+preformkmpm, but it told me that apache2 @2.2.17_1+preformkmpm
depends on itself, and that I should uninstall it. Obviously that wasn't
possible.

Finally I found the [migration guide][3] provided by the MacPorts website,
which instructed me to install the new MacPorts for OS X Lion before removing
and reinstalling the packages. I downloaded the DMG file for MacPorts
(MacPorts-2.0.3-10.7-Lion), installed it, and then the packages uninstalled
without any issues.

[1]: http://www.macports.org/
[2]: http://www.anthonymclin.com/code/7-miscellaneous/106-updating-xcode-and-macports-for-osx-lion
[3]: https://trac.macports.org/wiki/Migration
