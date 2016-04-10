---
layout: post
title: Getting Started with IRSSI
categories:
- Linux
tags:
- irc
---

Often open source projects or organizations use an IRC channel on FreeNode to
provide support to users and/or developers. I'm trying to retain familiarity
with the command line, rather than become completely dependent on GUI
applications, so I've decided to use [IRSSI](https://irssi.org/) instead of 
[Pidgin](https://www.pidgin.im/) or [Adium](https://adium.im/) (Mac OS X).

## Installation

Installing IRSSI is easy on Ubuntu, simply use:
```
apt-get install irssi
```

You can also install it using Homebrew on Mac OSX:
```
brew install irssi
```

After it's done installing, simply run the program
```
irssi
```

## Windows

IRSSI support separate "windows" for the different channels you are connected
to, or for the different people you are chatting with. If for some reason you
do not see information on the screen for a command you've run, it may be
displayed in another window.

By default you can use the ALT key combined with a number key (e.g. ALT+1,
ALT+2, ALT+3, etc) to switch between the different displays in IRSSI.

If your terminal program doesn't support this, or uses these key combinations
to switch between it's own tabs (like Ubuntu terminal does), then you should
use the `/window` command instead.

```
/WINDOW NEW                    - Create new split window
/WINDOW NEW HIDE               - Create new hidden window
/WINDOW CLOSE                  - Close split or hidden window

/WINDOW HIDE [<number>|<name>] - Make the split window hidden window
/WINDOW SHOW <number>|<name>   - Make the hidden window a split window

/WINDOW SHRINK [<lines>]       - Shrink the split window
/WINDOW GROW [<lines>]         - Grow the split window
/WINDOW BALANCE                - Balance the sizes of all split windows
```

## Commands

For those that are new to IRC, it's a good idea to become familiar with
the [common commands](http://www.ircbeginner.com/ircinfo/ircc-commands.html) 
that IRC programs support. Here are a few:

* `/join #channel_name` - Joins a channel
* `/me <action>` - Announces some action such as `/me waves hello`
* `/msg <nickname> <message>` - Send a direct message to another user
* `/ignore <nickname>` - Block someone that is harassing you
* `/quit` or `/exit` - Quits IRC program

There are other commands that are supported only by IRSSI that can be found in
the [IRSSI Documentation](https://irssi.org/documentation/)

It's recommended that you use the `/help` command to get the list of other
supported commands. You can get more information on each command by following
the `/help` command with the name of the command. For instance you can get more
information on the `/connect` command by using `/help connect`.

## Getting Started

Upon opening the program for the first time IRSSI will connect to a default
IRC network. There is a configuration file in ~/.irssi/config that you can
inspect, but you can use commands from within the program to configure IRSSI to
automatically perform when you first open the program. This includes connecting
to Freenode, authenticating using your registered nick name, and joining a
default channel.

You can use these commands to get started immediately:

Set your nick name and real name
```
/set nick <nick>
/set real_name <Real Name>
```

Connecting to FreeNode
```
/connect irc.freenode.net 8001
```

Join Channel
```
/join #ubuntu
```

## Registering with FreeNode

The FreeNode IRC network allows you to register your nickname and associate
it with your email address. This is done by using the following commands:

```
/msg nickserv REGISTER <password> <email>
```

You should receive a message informing you that you need to check your email
account and obtain instructions to verify yourself.

To make sure that your email address isn't revealed to other users, use the
following command to ensure that it is hidden.
```
/msg NickServ SET HIDEMAIL ON
```

You can verify your information with the NickServ by using:
```
/msg nickserv info
```

## Automatic Configuration

Configure IRSSI with Freenode network, then register the Freenode server you 
will connect to via an SSL connection, then configure the automatically joined
channel (#ubuntu in this example).
```
/network add Freenode
/server add -auto -ssl -ssl_verify -ssl_capath /etc/ssl/certs -network Freenode irc.freenode.net 7000
/channel add -auto #ubuntu Freenode
/save
```

After you've successfully registered your FreeNode nick name, you can run this
command to configure IRSSI to login automatically after connecting to FreeNode.

```
/network add -autosendcmd "/msg nickserv identify <password> ;wait 2000" Freenode
```
