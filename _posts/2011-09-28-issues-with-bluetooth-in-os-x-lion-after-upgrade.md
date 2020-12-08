---
layout: post
status: publish
published: true
title: Issues with Bluetooth in OS X Lion after Upgrade
author: maxwell keyes
date: '2011-09-28 19:08:39 -0700'
comments: true
categories:
- computing tips
---

I recently upgraded from OS X Lion (10.7.1) from Snow Leopard so that I could
be up-to-date and benefit from any good features. Truthfully I liked the old
Expose and Spaces better than this new Mission Control interface for virtual
desktops...but oh well it will do.

After upgrading to Lion the one thing that has been most frustrating is that
my Bluetooth headset doesn't work anymore. It syncs up with my MacBook Pro,
but it doesn't send audio from the mic nor does it playback audio. I'm using a
[VXI BlueParrott B250-XT][1].
<!--more-->

I'm referring to the instructions on [this page][2] to resolve the issue which
are:

* Don't have Power Management configuration issue causing bluetooth option to
not be present.
* Unplugged USB devices
* Ran Applications > Utilities > Disk Utility and ran a 'Repair Disk
Permissions' on my primary drive.
* Deleted user preference files for Bluetooth
(~/Library/Preferences/com.apple.Bluetooth and
~/Library/Preferences/com.apple.Bluetooth.plist)
* Reset PRAM
* Reset Power Management System
* Opened Bluetooth Explorer -> Utilities Menu -> Modify Software and
Device Configuration -> Checked:
  * 'Recant' Connected Apple HID Devices
  * 'Full Factory Reset' connected Apple HID devices
  * Delete link keys from the Bluetooth module
  * Delete global bluetooth preference file (/L/P/com.apple.Bluetooth.plist)
  * Remove all favorite devices
  * Unconfigure all 'configured' devices
  * Restart the Bluetooth daemon ('blued') process

This all did not resolve my issue. I then thought to myself, well...the device
is pairing and connecting. Perhaps it's a sound issue. I then proceeded to
find some sort of forum post or article explaining how to reset the
sound/audio settings which might be corrupted since upgrading to OS X Lion....
I'll update this post once I troubleshoot further.

[1]: http://www.vxicorp.com/products/blueparrott-bluetooth-mobile-solutions/bluetooth-headsets/b250-xt/
[2]: http://ismashphone.com/2011/08/how-to-fix-bluetooth-pairing-in-mac-os-x-lion-issues.html
