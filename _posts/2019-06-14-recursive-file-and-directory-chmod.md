---
layout: post
published: true
title: Fixing file and directory permissions recursively
description: Command to fix file and directory permissions
image_url: /images/posts/2019-06-14-linux.jpg
date: 2019-06-14 21:27:00 -0800
comments: true
categories:
  - linux
  - unix
tags:
  - cmdline
  - linux
---

Often I find myself downloading ZIP files, and after unarchiving them all the
files have totally incorrect permissions such as 777 for all files and folders.
<!--more-->

Go into the directory main directory at the top of the file/folder hierarchy
and run the following commands to resolve this:

```bash
find . -type d -exec chmod 0755 {} \;
find . -type f -exec chmod 0644 {} \;
```
