---
layout: post
title: Uninstalling Command Line Tools for Xcode
date: '2013-05-20 20:51:42 -0700'
categories:
- Mac OS X
tags:
- xcode
comments: []
---
I ran into a problem trying to install Ruby 2.0.0 via RVM over the weekend. When I got to work the next day and needed to do work using Ruby 1.8.7, I ran into issues. This led to updating RVM using 'rvm get stable', and then trying to reinstall Ruby 1.8.7.

I was then prompted that the version of Xcode I'm using is older, and thus needs to be updated. I tried to look for the command to uninstall it, just to ensure that the new installation (possibly via the App Store) is clean. I found this command to work for Mac OSX 10.8.3 (Mountain Lion).

``` shell
sudo /Library/Developer/4.1/uninstall-devtools -mode=all
```

If you have a much older version of XCode, the command might be:

``` shell
sudo /Developer/Library/uninstall-devtools --mode=all
```

Also, you might have a full version of XCode installed under /Applications/Xcode.app/, in which case you'll want to simply drag and drop it into your trash to uninstall it. I did this and RVM finally indicated that there was no remaining outdated version of XCode. As you can see it defaults by checking in the /Applications/Xcode.app/ folder for developer tools.

``` shell
Installing requirements for osx, might require sudo password.

Error: No developer directory found at /Applications/Xcode.app/Contents/Developer. Run /usr/bin/xcode-select to update the developer directory path.
```

Strange thing is that for me, even after reinstalling the Command Line Tools for XCode 4.6.2 and then restarting my machine, I still am receiving this error:

``` shell
Error: No developer directory found at /Applications/Xcode.app/Contents/Developer. Run /usr/bin/xcode-select to update the developer directory path.

Already up-to-date.
```

I'm not really sure what the new path is. I've found that it's best to simply install the full Xcode, so I've done that. No further errors regarding the developer directory path.

 

 

 

