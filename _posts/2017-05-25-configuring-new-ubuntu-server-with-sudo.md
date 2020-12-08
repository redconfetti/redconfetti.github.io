---
layout: post
title: Configuring a New Ubuntu Server with Sudo
date: 2017-05-25 21:08:00 -0700
categories:
- linux
tags:
- ubuntu
- sudo
- sshd
- security
---

Here are my notes for configuring a new Ubuntu server with a single user with
sudo rights, with the 'root' user login disabled in the SSHd configuration.

This guide assumes that you have just created a server from the web interface of
a service like [Linode] or [Digital Ocean], and you know the root password.

[Linode]: http://www.linode.com
[Digital Ocean]: http://www.digitalocean.com
<!--more-->

## Local Configuration

From your local machine, you can configure your SSH client within
`~/.ssh/config`. Use the following configuration to connect to the server using
a specific username and SSH key.

```SSH Config
Host myserver
  Hostname 192.168.1.2
  Port 22
  User johnsmith
  IdentityFile ~/.ssh/id_rsa
```

This configuration makes it possible to connect to the server quickly using
`ssh myserver`.

## Create User Account

Because the account doesn't exist on the server yet, you'll need to login as
root with the root password or with the same type of SSH key configuration.

Once you're in the server as 'root', create the user account using:

``` shell
adduser johnsmith
```

## Add User to Sudo Group

Ubuntu has a 'sudo' group already setup. Simply use `usermod` to add your new
user account to that group.

``` shell
usermod -aG sudo johnsmith
```

## Configure SSH Key

You'll need to login to the new account using the `su` command.

``` shell
su - johnsmith
```

Next create a `~/.ssh` folder with `authorized_keys` file, and place your public
SSH key (likely located within `~/.ssh/id_rsa.pub` on your local machine) within
this `authorized_keys` file.

``` shell
mkdir -p ~/.ssh/
touch ~/.ssh/authorized_keys
nano ~/.ssh/authorized_keys
```

After you've configured your new account for SSH key authentication, use `exit`
to get back to 'root'.

## Configure SSHd to Disallow Root User Login

Within `/etc/ssh/sshd_config`, find the `PermitRootLogin` setting and set it
from 'yes' to 'no'. Do the same for the 'PasswordAuthentication' setting.

```ssh config
PermitRootLogin no
PasswordAuthentication no
```

Also add an entry that ensures that only your sudo user can login, for good
measure. This keeps any other accounts that might exist from being used.

```shell
AllowUsers johnsmith
```

For good measure you could also change the port used by SSHd. This would require
that you set a different port number in your SSH config file (`~/.ssh/config`)
with the port specified.

```shell
Port 6221
```

After you've saved the changes to the configuration file, restart the SSHd server.

```shell
/etc/init.d/ssh restart
```

Now logout and hope that you can login as your user. Perhaps you should stay
logged in and open a new terminal tab and test it out before you logout as
'root'.

## Becoming Root

Now you can log into your box as your normal user account, then become root
using:

```shell
sudo su root
```

You'll just have to type in your own password once. This means that hackers will
need to know your username, SSH private key, and password, to gain full access
to your box.
