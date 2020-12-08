---
layout: post
title: Intro to Tmux
categories:
- Linux
tags:
- tmux
- screen
---

Recently I learned a few of the basic commands needed to use
the GNU [screen](https://en.wikipedia.org/wiki/GNU_Screen) command to keep
a command line session running even after I've disconnected from a remote VPS.
I learned this specifically so that I could keep [irssi](https://irssi.org/)
running and logged into a specific IRC channel, so I could return to the session
and view the history of messages that I had missed.

Recently I heard about [Tmux](https://en.wikipedia.org/wiki/Tmux) as an
alternative solution, and also discovered that it can also be used to maintain
separate virtual terminals (windows), as well as split the screen into separate
"panes". Splitting the screen into panes can also be done with GNU screen, but
it's not as well supported. See [reasons to use tmux instead of screen](http://superuser.com/questions/236158/tmux-vs-screen).
<!--more-->

## Installation

### Installing for Mac

Use [Homebrew](http://brew.sh/) to install tmux on a Mac OS X machine.

``` shell
brew install tmux
```

### Installing for Linux

``` shell
sudo apt-get install tmux
```

## First Use

After you run `tmux` for the first time, you'll notice that you are returned to
a typical shell prompt, however there is now a green bar at the bottom of your
screen.

You are now operating within a tmux session. Within a session you can establish
multiple windows, with each window supporting multiple panes that display within
the window.

Sessions are like different work spaces. You can detach from a work space and
then drop into another session that you setup previously.

### Panes

Many commands supported by tmux involve using a keystroke known as the 'prefix'.
By default this keystroke is CTRL + B, however you can configure tmux to use
a different keystroke as the prefix.

Pressing CTRL + B, followed by `%` will split the screen into two panes
vertically. You can press CTRL + B (PREFIX), then one of the arrow keys to
switch between the panes. For instance PREFIX, then LEFT ARROW key will move
your cursor to the left pane.

Next if you type PREFIX then `"` (`PREFIX - "`), it will split the pane into two
panes horizontally. You can use PREFIX then UP ARROW or DOWN ARROW to switch
between the horizontal panes.

When you are inside of a specific pane, you can hold down the PREFIX keystroke
and tap one of the arrow keys multiple times to resize the pane.

If you want to make a pane temporarily full screen, you can use `PREFIX - z` to
toggle between full screen and original size.

To close a pane you can simply use the `exit` command from the shell.

### Windows

Use `PREFIX - c` to open up a new window. You'll now see that the status bar
at the bottom of the screen reflects the new windows existence. You can switch
between the windows using PREFIX followed by the number of the window (e.g.
`PREFIX - 0` and `PREFIX - 1`).

Notice that there is an asterisk (*) next to the window that you are currently
viewing in the status bar, and a dash (-) next to the last window you were
viewing. `PREFIX - l` will allow you to switch between the current and last
window.

### Sessions

A session will run in tmux until you end it. You can detach from a session by
using `PREFIX - d`. You can list sessions using `tmux list-sessions` or
`tmux ls`.

You can re-attach to the tmux session by using `tmux attach`. If you have
multiple sessions runing, you can use `tmux attach -t 0`, where `-t`
means "target", and `0` is the number for the session. You can also kill
sessions using a similar command: `tmux kill-session -t 0`.

Alternatively you can name sessions when you start them using
`tmux new -s session_name`, and then use `tmux attach -t session_name` to
reconnect to the session.

## Further Reading

* [tmux cheatsheet](/resources/cheat-sheets/linux/#tmux)
* [Thoughtbot - A tmux Crash Course](https://robots.thoughtbot.com/a-tmux-crash-course)
* [The Pragmatic Programmer - tmux](http://pragprog.com/book/bhtmux/tmux)
