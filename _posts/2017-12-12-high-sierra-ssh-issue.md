---
layout: post
published: true
title: SSH issues with Mac OS X High Sierra
description: How to resolve
image_url: /images/posts/2017-12-12-macos-high-sierra.jpg
date: 2017-12-12 11:08:00 -0700
comments: true
categories:
- macosx
tags:
- high sierra
- ssh
---

A coworker of mine was reporting an issue with SSH after updating to Mac OS X
High Sierra.

```shell
$ ssh server-alias-hostname
Unable to negotiate with 192.168.1.5 port 22: no matching cipher found. Their offer: blowfish-cbc,aes256-cbc
```

You can view a list of supported ciphers by running `ssh -Q cipher`.
<!--more-->

It turns out that the system is configured to use certain ciphers within
`/etc/ssh/ssh_config`. You can adjust your local configuration within
`~/.ssh/config` to make sure that the ciphers supported by your local client
match one of the ones offered by the remote server.

```SSH Config
# ~/.ssh/config
Host *
  SendEnv LANG LC_*
  Ciphers +aes256-cbc
```
