---
layout: post
status: publish
published: true
title: Selenium - no display specified
author: maxwell keyes
date: '2010-01-11 14:51:10 -0800'
date_gmt: '2010-01-11 18:51:10 -0800'
categories:
- computing tips
tags: []
comments:
- id: 702
  author: Nigel Horne
  author_email: njh@bandsman.co.uk
  author_url: ''
  date: '2010-02-25 13:32:41 -0800'
  date_gmt: '2010-02-25 17:32:41 -0800'
  content: "I tried that, but I still get the same error:\r\n\r\n\r\n    Error: no
    display specified"
- id: 703
  author: Nigel Horne
  author_email: njh@bandsman.co.uk
  author_url: http://www.bandsman.co.uk
  date: '2010-02-25 13:44:41 -0800'
  date_gmt: '2010-02-25 17:44:41 -0800'
  content: "Sorry, wrong error message. What I see *after* implementing your suggestion
    is\r\n\r\nError: cannot open display: :0"
- id: 711
  author: Onlineproxy
  author_email: admin@proxydienst.de
  author_url: http://www.proxydienst.de
  date: '2010-04-07 10:02:24 -0700'
  date_gmt: '2010-04-07 14:02:24 -0700'
  content: "hello.\r\ni ve got the same problem.\r\n\r\nError: no display specified\r\nwhen
    try to start firefox via selenium.\r\n\r\n\r\nseems to be something with $DISPLAY???"
- id: 743
  author: kzg
  author_email: kzg@xc.hu
  author_url: http://xc.hu
  date: '2010-05-08 16:23:22 -0700'
  date_gmt: '2010-05-08 20:23:22 -0700'
  content: "export DISPLAY=:0\r\nthis saved me a lot of time, thx"
- id: 787
  author: Jeevan Pingali
  author_email: jeevan.pingali@gmail.com
  author_url: ''
  date: '2010-06-14 09:34:33 -0700'
  date_gmt: '2010-06-14 13:34:33 -0700'
  content: It definitely helped me, thank you
- id: 867
  author: SeanG
  author_email: sean.gilbertson@gmail.com
  author_url: http://twitter.com/parsifal
  date: '2010-07-20 21:06:32 -0700'
  date_gmt: '2010-07-21 01:06:32 -0700'
  content: This definitely helped me. Thanks!!
- id: 888
  author: David Miller
  author_email: david@deadpansincerity.com
  author_url: http://deadpansincerity.com
  date: '2010-07-30 07:49:37 -0700'
  date_gmt: '2010-07-30 11:49:37 -0700'
  content: Many thanks, spent the best part of a morning not thinking of this :)
- id: 1551
  author: tejas
  author_email: tejas.sarade@gmail.com
  author_url: ''
  date: '2011-02-03 15:15:13 -0800'
  date_gmt: '2011-02-03 19:15:13 -0800'
  content: After adding env variable DISPLAY don't run firefox as a root.
- id: 1562
  author: Sami R.
  author_email: ranezu@hotmail.com
  author_url: ''
  date: '2011-02-10 23:51:22 -0800'
  date_gmt: '2011-02-11 03:51:22 -0800'
  content: Just what I needed, thanks!
- id: 1613
  author: goph-R
  author_email: gopher@vipmail.hu
  author_url: http://dynart.info
  date: '2011-02-24 11:36:26 -0800'
  date_gmt: '2011-02-24 15:36:26 -0800'
  content: Thanks man! It is very helpful!
- id: 1931
  author: Ansuman
  author_email: ansuman.01@gmail.com
  author_url: ''
  date: '2011-07-04 08:44:20 -0700'
  date_gmt: '2011-07-04 12:44:20 -0700'
  content: "I am facing a different problem.\r\n\r\nwhen I try to run a code as JUnit
    test case,\r\n\r\nit gives me a message like this in command prompt.\r\n\r\n\r\n11:20:41.382
    INFO - Command request: getNewBrowserSession[*firefox, http://www.l\r\nogica.com/,
    ] on session null\r\n11:20:41.382 INFO - creating new remote session\r\n11:20:41.384
    INFO - Allocated session c3a6b2920e0e42289266f5e132dc0593 for http:\r\n//www.logica.com/,
    launching...\r\n11:20:46.268 INFO - Preparing Firefox profile...\r\n\r\n\r\nit
    stays there forever. does not do anything. \r\nthen I created my own firefox profile,
    but still the same problem.  cant understand whats the problem really !!\r\n\r\nSomeone
    please help. !!!!\r\n\r\nthanks"
- id: 2118
  author: dom
  author_email: dom@megaflea.net
  author_url: http://megaflea.net
  date: '2011-10-14 12:11:04 -0700'
  date_gmt: '2011-10-14 16:11:04 -0700'
  content: awesome! thankyou!, I searched for ages but my search stopped here :D
- id: 2234
  author: tojo
  author_email: tajohn77@yahoo.com
  author_url: ''
  date: '2011-12-19 17:39:12 -0800'
  date_gmt: '2011-12-19 21:39:12 -0800'
  content: Thanks for posting this, saved me tons of time!
- id: 2409
  author: Evgenia
  author_email: mevgenia@gmail.com
  author_url: ''
  date: '2012-03-29 06:01:21 -0700'
  date_gmt: '2012-03-29 10:01:21 -0700'
  content: "I was facing this problem:\r\norg.openqa.selenium.WebDriverException:
    Error forwarding the new session new session request for webdriver should contain
    a location header with the session.\r\nBuild info: version: '2.20.0', revision:
    '16008', time: '2012-02-28 15:00:40'\r\nSystem info: os.name: 'Linux', os.arch:
    'amd64', os.version: '2.6.38-8-generic', java.version: '1.6.0_22'\r\nDriver info:
    driver.version: RemoteWebDriver\r\n\r\nand the solution you wrote down helped
    me to resolve the problem. Thanks a lot!"
- id: 2428
  author: lukhwa
  author_email: argusianus_argus@hotmail.com
  author_url: ''
  date: '2012-04-14 04:50:58 -0700'
  date_gmt: '2012-04-14 08:50:58 -0700'
  content: Thanks! It works :)
- id: 2653
  author: Ivan
  author_email: icevan@ngs.ru
  author_url: ''
  date: '2013-01-21 04:05:42 -0800'
  date_gmt: '2013-01-21 08:05:42 -0800'
  content: THANK YoU!
- id: 3419
  author: Sudhanshu
  author_email: hi.sudhanshu02@gmail.com
  author_url: ''
  date: '2013-06-07 03:29:26 -0700'
  date_gmt: '2013-06-07 07:29:26 -0700'
  content: "I tried the solution, but it shows the following error. \r\n\r\nError:
    cannot open display: :0\r\n\r\nPlease help !!"
- id: 3865
  author: jita32
  author_email: chiangtor@gmail.com
  author_url: ''
  date: '2013-07-05 04:36:24 -0700'
  date_gmt: '2013-07-05 08:36:24 -0700'
  content: tks. that's works for me.Ubuntu12.04lts.
---

I'm not very experienced with X-windows on the Linux platform, so I'm not too skilled in troubleshooting issues with the display. I recently upgraded an Ubuntu system at work to use Ubuntu 9.04 (Jaunty Jackalope), which only has Firefox 3 available (no package for Firefox 2). I had a Selenium server setup running tests, but they stopped working after I upgraded to this newer version of Ubuntu.

I thought that perhaps Selenium wasn't compatible with version 3 of Firefox, but this isn't the case. The Selenium website says ['Firefox 2+' for browsers running on Linux](http://seleniumhq.org/about/platforms.html#operating-systems).

The error I was receiving when I would run a test was:
```
10:46:19.778 INFO - Preparing Firefox profile...
Error: no display specified
```

After a bunch of research and Googling online, it turned out I just needed to run this before I started my Selenium server:
```
export DISPLAY=:0
```

I hope this saves someone else some time.
