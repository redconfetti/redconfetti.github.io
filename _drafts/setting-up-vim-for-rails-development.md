---
layout: post
title: Setting up VIM for Rails Development
categories:
- Development
tags:
- vim
---
I'm getting the impression that many Rubyist's, including Rails developers, are finding that they like using Vim as their text editor, thus relying on a command line console solely as their IDE. One of the reasons for this is that Vim is available by default on most platforms, and has been ported to most. Many have attested that although Vim has a steep learning curve, once you master it you will appreciate the power it provides. For me, it just makes sense to learn how to use this tool locally so that I can also use it remotely on production or staging servers when I need to troubleshoot an issue that cannot be reproduced locally. 

## Install / Update Homebrew

I used to use MacPorts to install Unix-based software ports on my Mac, but I've found that I have less issues with [Homebrew](http://mxcl.github.com/homebrew/). As an example, I would at times have issues with the installation of certain packages due to permission issues. Homebrew installs software local to your user, so you never run into complications where you have to use sudo to install something. Usually you only need to install the Command Line Tools for XCode package to install packages with Homebrew, but MacVim requires the full XCode installation. In fact, after installing the full XCode, I was prompted to also install the command line tools. [https://developer.apple.com/downloads/index.action](http://www.google.com/url?q=https%3A%2F%2Fdeveloper.apple.com%2Fdownloads%2Findex.action&sa=D&sntz=1&usg=AFrqEzc3T55YBcKQro23-0tfHsVV6aiZdQ)

``` shell
$ brew doctor
Warning: Experimental support for using Xcode without the "Command Line Tools".
You have only installed Xcode. If stuff is not building, try installing the
"Command Line Tools for Xcode" package provided by Apple.
```

After installing XCode by dragging it from the DMG package to my 'Applications' folder, I ran into issues with Homebrew installing MacVim still. You may need to run this command to configure the proper Xcode path.

``` shell
sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer
```

## Install MacVim

Okay, maybe we don't want MacVim. [http://www.thisisthegreenroom.com/2011/making-the-jump-textmate-to-vim/](http://www.thisisthegreenroom.com/2011/making-the-jump-textmate-to-vim/)

``` shell
brew install macvim
```

## Install Vim-Ruby

Next...

## Vim Pack

[https://github.com/bramswenson/vimpack](https://github.com/bramswenson/vimpack)

## Learning Vim

vimtutor
