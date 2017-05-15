---
layout: post
status: publish
published: true
title: Minecraft Mods
date: '2013-02-10 02:30:53 -0800'
date_gmt: '2013-02-10 06:30:53 -0800'
categories:
- gaming
tags:
- minecraft
- mods
- craftbukkit
- console
---

I've been playing [Minecraft](http://minecraft.net/) for a little while now. Want to see what else this thing can do, so I want to get access to console commands. This is a challenge it seems, so I'm going to document how it's done here.

I'm currently using Minecraft version 1.4.7. I just downloaded the latest build/snapshot (possibly unstable yet likely compatible) from [Minecraft-Console](https://github.com/simo415/Minecraft-Console) by simo415 on Github. A prerequisite to this mod running is [modloader](http://www.minecraftforum.net/topic/75440-v147-risugamis-mods-updated/), which states that it's only for version 1.4.7.

I'm using a Mac. I downloaded the [modloader](https://dl.dropbox.com/u/3321707/software/Minecraft/ModLoader.zip) source files, then followed the instructions to unpack the contents of the minecraft JAR file into a temporary directory.

``` shell
cd ~
mkdir mctmp
cd mctmp
jar xf ~/Library/Application\ Support/minecraft/bin/minecraft.jar
```

Next I copied the contents of the modloader source files into the temporary directory, overwriting all files. I then ran the following to remove some files and repackage the minecraft JAR file.

``` shell
rm META-INF/MOJANG_C.*
jar uf ~/Library/Application\ Support/minecraft/bin/minecraft.jar ./
cd ..
rm -rf mctmp
```

It wasn't clear how I install the Minecraft Console mod after performing this, but another website helped me see that I simply needed to drop the `Minecraft_Console_Snapshot.zip` file into `~/Library/Application Support/minecraft/mods`, which I renamed as 'Minecraft_Console.zip' and restarted the program. I could see that in that directory a 'console' folder showed up with configuration files and logs, so I knew the plugin was loading.

The 'GuiAPI' plugin is optional, and it required some sort of 'Forge' software to be present, so I decided not to explore this. I obtained the `SinglePlayerCommands-MC1.4.7_V4.5.jar` and ran the file, which presented an installer that auto-detected where to install the files. I did this and restarted the program, but the command's don't seem to work. I really wanted this because it seems to provide commands that are very helpful from the perspective of a single player... like reporting where my position is using '//pos' (which didn't work).

With Minecraft Console installed, I can use the backslash key to bring up the console, however I find that I can simply use the `t` command to talk like normal...and this still respects commands starting with forward slash such as `/time set 0`. I also found that the forward slash key also brought up a nice looking console that was large yet less intrusive to the game.

The time set command didn't work however, but instead responded with "You don't have permission to set the time". I connected to my server using RemoteBukkit and used '/op myusername' to give myself Op privileges. After doing this I was able to set the time, and other commands.

