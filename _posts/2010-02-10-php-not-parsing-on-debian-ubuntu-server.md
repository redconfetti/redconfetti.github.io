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
comments:
- id: 793
  author: Dante Elrik
  author_email: elrikdante@elrikgroup.com
  author_url: http://ElrikGroupCo
  date: '2010-06-22 23:14:47 -0700'
  date_gmt: '2010-06-23 03:14:47 -0700'
  content: "That was very helpful; I'd been trying to figure this issue out for nearly
    an hour before I stumbled upon your site.\r\n\r\nThanks"
- id: 799
  author: admin
  author_email: jason@redconfetti.com
  author_url: http://
  date: '2010-06-25 01:56:59 -0700'
  date_gmt: '2010-06-25 05:56:59 -0700'
  content: Thats why I posted it. I'm glad it helped you fix it instead of taking
    many hours more.
- id: 933
  author: Flic
  author_email: flickyll@yahoo.com
  author_url: ''
  date: '2010-08-15 00:59:08 -0700'
  date_gmt: '2010-08-15 04:59:08 -0700'
  content: Thank you very much!  I have been tearing my hair out for hours (not to
    mention uninstalling and reinstalling software) trying to work this out and although
    your solution didn't exactly match my problem, it told me where I should be looking.  Pretty
    straightforward from there.  Now if only Google would return this higher up in
    its results instead of 20 sites giving the same installation instructions...
- id: 1095
  author: eZH
  author_email: emakhrov@gmail.com
  author_url: http://ezh.net.ru
  date: '2010-10-29 01:43:15 -0700'
  date_gmt: '2010-10-29 05:43:15 -0700'
  content: "Thanks a lot.\r\ndamn packagers, they ruin everything"
- id: 1108
  author: TonyC
  author_email: t@tcwebadmin.com
  author_url: http://www.tcwebadmin.com
  date: '2010-11-04 04:39:03 -0700'
  date_gmt: '2010-11-04 08:39:03 -0700'
  content: I love you!  I've been scouring all over the internet looking for this
    bugger!  Like I somehow had php running correctly one one of my vhosts but when
    my client wanted to get add more sites, I ran into this problem again.. Darn just
    knew it had to be a bug with the version of Ubuntu!
- id: 1939
  author: dh
  author_email: dh@idr.cz
  author_url: http://david.hlouch.cz/
  date: '2011-08-02 00:43:02 -0700'
  date_gmt: '2011-08-02 04:43:02 -0700'
  content: "After long search and a lot of tries, this worked for my sandbox vserver
    as well. Glad I don't have to bother our admin again. :)\r\nThanks a lot guys.
    Good job."
---

My friend Marshall was recently having issues getting PHP5 installed and working on his Ubuntu server, which is a Debian based distribution.

We updated all the packages involved...Apache2, php5, libapache2-mod-php5, made sure the module was installed, restarted Apache2, etc. Nothing worked.

It turns out that the default php5.conf configuration for Debian / Ubuntu's packages are using an incorrect syntax. Edit /etc/apache2/mods-available/php5.conf to reflect:

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

[http://wiki.apache.org/httpd/DebianPHP](http://wiki.apache.org/httpd/DebianPHP)

I'm just posting this solution here for all the other nerds having the same issue that aren't finding this article in Google.

