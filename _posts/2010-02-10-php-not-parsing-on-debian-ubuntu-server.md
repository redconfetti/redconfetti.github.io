---
layout: post
status: publish
published: true
title: PHP Not Parsing on Debian / Ubuntu server with Apache2
author: maxwell keyes
date: '2010-02-10 21:03:59 -0800'
date_gmt: '2010-02-11 01:03:59 -0800'
categories:
- web development
---

My friend Marshall was recently having issues getting PHP5 installed and
working on his Ubuntu server, which is a Debian based distribution.

We updated all the packages involved...Apache2, php5, libapache2-mod-php5,
made sure the module was installed, restarted Apache2, etc. Nothing worked.

It turns out that the default php5.conf configuration for Debian / Ubuntu's
packages are using an incorrect syntax. Edit /etc/apache2/mods-available/php5
conf to reflect:

```
<FilesMatch \.php$>
  SetHandler application/x-httpd-php
</FilesMatch>
```
...instead of...

```
AddType application/x-httpd-php .php
```

Special thanks to this Apache wiki article for pointing this out:

http://wiki.apache.org/httpd/DebianPHP

I'm just posting this solution here for all the other nerds having the same
issue that aren't finding this article in Google.
