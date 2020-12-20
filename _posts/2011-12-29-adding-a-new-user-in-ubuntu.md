---
layout: post
title: Adding a New User in Ubuntu
date: '2011-12-29 18:08:48 -0800'
comments: true
categories:
- Hosting
---

## Adding User

When setting up a new website manually on an Ubuntu server you need to
establish a user account with a home directory, and Bash shell access to the
server.

``` shell
useradd -m testuser -s /bin/bash
```

After creating the account you'll want to assign a password for the account.

``` shell
passwd testuser
```
