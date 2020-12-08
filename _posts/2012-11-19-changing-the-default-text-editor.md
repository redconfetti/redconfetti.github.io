---
layout: post
title: Changing the Default Text Editor
date: '2012-11-19 21:30:07 -0800'
categories:
- Mac OS X
tags:
- git
- text-editor
comments:
- id: 15311
  author: Exploring Bundler Commands | Ruby Colored Glasses
  author_email: ''
  author_url: http://www.rubycoloredglasses.com/2013/08/exploring-bundler-commands/
  date: '2013-09-01 18:38:44 -0700'
  date_gmt: '2013-09-01 18:38:44 -0700'
---

Certain command line utilities drop into an external text editor program to
accept certain types of input. For instance, when using the command
'crontab -e' to edit your cron table, your default text editor program will be
opened up with the current cron table configuration. The same also applies to
the Git versioning system when using the [interactive rebase mode]. This helps
the program avoid supporting it's own text editor, and allows the user to
specify their preferred text editor.

To specify the default text editor, simply edit or place the following
definition inside of the .bash_profile file in your home directory. This example
uses '/usr/local/bin/mate -w' to specify that the Textmate editor be used.
You may configure this value to reflect the path for Vim, Nano, or any other
text editor you wish to use.
<!--more-->

```shell
export EDITOR="/usr/local/bin/mate -w"
```

It's also possible to explicitly [configure Git to use a specific text editor],
thus overriding the default 'EDITOR' value specified in the command line
environment. This is useful if you only want to change the behaviour of Git, and
not affect the rest of your environment.

```shell
git config --global core.editor "mate -w"
```

----

UPDATE - 03/28/2013:

I recently switched to [Sublime 2] text editor. After installing the application
I created a symlink like so:

```shell
ln -s "/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl" /usr/local/bin/subl
```

After this was completed I added the following to my shell config file
(.bash_rc / .zshrc):

```bash
# Text Editor
export EDITOR=/usr/local/bin/subl
```

If you plan on using Sublime with utilities that expect you to save and close
the file before the utility continues, you'll need to configure a subl_wait
script as outlined [here].

To use Sublime Text with Git during processes like an interactive rebase,
configure it as the text editor using this command:

```shell
git config --global core.editor "subl -n -w"
```

[interactive rebase mode]: http://git-scm.com/docs/git-rebase#_interactive_mode
[configure git to use a specific text editor]: http://git-scm.com/book/en/Customizing-Git-Git-Configuration#Basic-Client-Configuration
[sublime 2]: http://www.sublimetext.com/
[here]: http://sublimetext.userecho.com/topic/91740-equivalent-of-mate_wait-for-subl/
