---
layout: page
title: Vim
---

[Back to Cheat Sheets](/resources/cheat-sheets/)

## Basics

Vim has 3 modes (Normal Mode, Insert Mode, Line Mode). When you enter into Vim,
you're automatically placed into Normal mode, also known as Command mode.

From Normal mode, press 'i' to go into insert mode to enter text content. Press
ESC to go back into Normal mode.

Press ':' to go into Line mode from Normal mode. Press ESC to go back into
Normal mode. Use 'q' from line mode to quit Vim. If you have made changes to the
currently loaded file, it will ask you to save changes. You can use 'q!' to quit
without saving the changes. You can use 'wq' to save the changes and quit at the
same time.

## Normal Mode

This mode is intended mostly for navigating and using commands to modify the
file.

### Navigation

These are the same as using the arrow keys.

- j - move down a line
- k - move up a line
- l - move to the right
- h - move to the left

Jump between words

- w - move to next word
- W - move to next word (ignore punctuation)
- b - move to previous word
- B - move to previoius word (ignore punctuation)

Jumping to lines

- gg - Jump to beginning of file
- G - Jump to end of file
- Line number followed by 'gg' or 'G' (e.g. '22g' or '22G')

Jumping on a line

- 0 - Move to beginning of a line
- ^ - Move to beginning of a line
- \$ - Move to end of a line

These are the same as using Page-Up and Page-Down keys, if present.

- CTRL-F - move forward / page down
- CTRL-B - move backwards / page up

Status Line

- CTRL-g - display line with current status (file name, status, cursor position,
  total lines)
- g + CTRL-g - display line with columns, lines, words, and bytes

Shift viewport

- z-ENTER - Moves text at current cursor up

### Modification

- x - Delete (Cut) character under cursor
- X - Delete (Cut) character before cursor (to the left)
- dw - Delete (Cut) current word
- dd - Delete current line
- dl - Deletes the current character
- dh - Deletes character before cursor (to the left)
- dj - Deletes current line, and the one below it
- dk - Deletes current line, and the one above it
- d0 - Deletes from cursor to beginning of line
- d\$ - Deletes from cursor to end of the line
- D - Deletes from cursor to end of the line
- y - Yank (copy) current character
- yy - Yank (copy) current line
- yw - Yank (copy) current word
- y0 - Yank (copy) from cursor to beginning of line
- y\$ - Yank (copy) from cursor to end of the line
- p - Paste last deleted/yanked content after cursor
- P - Paste last deleted/yanked content before cursor
- u - Undo
- CTRL-R - Redo
- . - Issues the previous command

## Line Mode

These commands begin with ':' to go into line mode, followed by ENTER.

- wq - write quit
- q - quit
- q! - quit without saving
- 0-100000000 - moves to line number specified
- set ruler - display ruler

If you type a partial command, then press CTRL-D, you'll be shown a list of
suggested commands.

## Help

You can access the help by running 'help' or 'h' from the line mode.
You can also try to jump to a specific topic by typing a blurb after this
command (e.g. ':h dd' jumps to 'dd' command).

If you place your cursor over a help topic, you can press CTRL + ] to go into
that topic. Press CTRL + o to go back to your previous location.

## Searching

Taken from [Finding a Word in Vi/Vim].

To find a word in Vi/Vim, simply type the / or ? key, followed by the word
you're searching for.

Once found, you can press the n key to go directly to the next occurrence of the
word.

Vi/Vim also allows you to launch a search on the word over which your cursor is
positioned. To do this, place the cursor over the term, and then press \* or #
to look it up.

[finding a word in vi/vim]: http://ccm.net/faq/982-finding-a-word-in-vi-vim
