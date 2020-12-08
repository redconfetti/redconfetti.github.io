---
layout: post
published: true
title: Ubuntu 9.10 Karmic Koala - VNC resolution limited without monitor
date: '2010-02-20 07:29:28 -0800'
date_gmt: '2010-02-20 11:29:28 -0800'
categories:
- Ubuntu
tags: []
---

__Update 04/23/2010__ - I'm not finding a solution to this issue. Sorry. I've
lost interest.

I recently setup Ubuntu 9.10 on a desktop system, so I could use it as a file
server. I'm was able to enable the remote desktop feature for it, which is
basically a VNC server.

The issue is that once I disconnected a monitor from the computer and set it
up next to my router (plugged directly in), and restarted it, VNC would only
work with a maximum resolution of 640x480.
<!--more-->

In a [forum] someone pointed out that this configuration added to
`/etc/X11/xorg.conf` would save the day:

```inf
Section "Device"
  Identifier "VNC Device"
  Driver "vesa"
EndSection

Section "Screen"
  Identifier "VNC Screen"
  Device "VNC Device"
  Monitor "VNC Monitor"
  SubSection "Display"
  Modes "1024x768"
  EndSubSection
EndSection

Section "Monitor"
  Identifier "VNC Monitor"
  HorizSync 30-70
  VertRefresh 50-75
EndSection
```

I just restarted after my 'sudo service gdm restart' command didn't seem to
work. I think this might have something to do with a special Nvidia driver I'm
using. Hm...

And I thought this [most recent article][webgroup west article] was giving me
the holy grail.

Eh. I'm going to bed now. It's 3:29 AM. I'll slay this dragon later perhaps.

[forum]: http://ubuntu-virginia.ubuntuforums.org/showthread.php?p=8636175
[webgroup west article]: http://webgroupwest.com/2010/01/fixing-screen-resolution-for-a-ubuntu-headless-vnc-server/
