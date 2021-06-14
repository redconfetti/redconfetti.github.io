---
layout: page
title: Linux
---

[Back to Cheat Sheets](/resources/cheat-sheets/)

Use the `man` command to read more about any of the following commands.

For example, you can read more about the `file` command by running `man file`.

## Misc commands

``` shell
# Discover a files type (text, executable, etc)
file /bin/bash

# Create Tar Gzip archive
tar -cvzf archive.tar.gz /path/to/folder/

# scan network for hosts
sudo nmap -sS -O -v 192.168.0/24

# View Manual Page for Executable Utility
man cp

# Search for Manuals by Keyword
man -k directories

# view calendar
cal
```

## grep

``` shell
# reveal 10 lines before, and 20 lines after matching line for context
grep -B10 -A20 'HTTP 404' /path/to/file
```

## top

``` shell
# view running processes, including threads
top -H
```

## sudo

``` shell
# list the sudo privileges for the current user
sudo -l
```

## tmux

### Commands

``` shell
# Create a named session
tmux new -s session_name

# List sessions
tmux list-sessions
tmux ls

# Attach to a session
tmux attach -t session_name

# List info
tmux info

# List tmux commands
tmux list-commands

# List configured key bindings and commands
tmux list-keys
```

## Key Bindings

* `CTRL + B` (the "PREFIX")
* `PREFIX %` - Split panes vertically
* `PREFIX "` - Split panes horizontally
* `PREFIX arrow_key` - Move to another pane
* `PREFIX + arrow_key` - Hold the prefix while pressing to resize pane
* `PREFIX z` - Toggle between pane and fullscreen
* `PREFIX c` - Open a new window
* `PREFIX number_key` - Switch to a window by number
* `PREFIX l` - Toggle between current and last window
* `PREFIX d` - Detach from session
