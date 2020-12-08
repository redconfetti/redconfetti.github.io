---
layout: post
published: true
title: Fixing audio for Steam (Rust) on Mac OS X Mojave
description: Hacking Steam to have Microphone Permissions
image_url: /images/posts/2019-11-07-rust.jpg
date: 2019-11-07 23:15:00 -0800
categories:
  - macosx
tags:
  - rust
  - steam
  - mojave
---

Recently I bought a new Macbook Pro running Mojave. I found myself
unable to get the microphone to work for the in-game voice chat using
the 'v' key.

The closest solution I could find was mentioned in this article:

- [How to Fix Voice Chat in Mac OS Mojave](https://www.reddit.com/r/leagueoflegends/comments/ay9o4s/how_to_fix_voice_chat_in_macos_mojave/)

This article was oriented towards League of Legends, so I had to modify the
commands used to enable this for steam.
<!--more-->
## Disable Protection

You still have to reboot the Mac while holding Command + R during start up.
In the recovery mode you'll have to use the menu to run the Terminal, and
then run `csrutil disable`.

After this is completed, reboot the computer.

## Run Commands

```shell
sudo sqlite3 ~/Library/Application\ Support/com.apple.TCC/TCC.db "INSERT or REPLACE INTO access VALUES('kTCCServiceMicrophone','com.valvesoftware.steam',0,1,1,NULL,NULL,NULL,'UNUSED',NULL,0,1551892126);"

/usr/libexec/PlistBuddy -c "Add NSMicrophoneUsageDescription string" /Applications/Steam.app/Contents/Info.plist

/usr/libexec/PlistBuddy -c "Set :NSMicrophoneUsageDescription Using voice chat" /Applications/Steam.app/Contents/Info.plist
```

## Re-enable Protection

Reboot into recovery mode again, open the Terminal and run
`csrutil enable`. Restart once again.

After doing this, the Rust game was able to transmit my voice from the mic.
