---
layout: post
published: true
title: Installing PHPdoc for Ubuntu for use with Command Line
date: '2011-05-18 20:21:45 -0700'
date_gmt: '2011-05-19 00:21:45 -0700'
categories:
- web development
---

I wanted to install PhpDocumentor for use on my server so that I could
generate documentation from the command line. I found [this article][1], which
instructed me to somehow change the PEAR setting for data_dir. I installed
PhpDocumentor in my web root and it just didn't work and gave me a bunch of
errors in the browser.

I uninstalled the package via PEAR (ala 'pear uninstall phpdocumentor'
command), then deleted the folder. I just wanted to install it in the proper
path so that it's available from the command line using 'phpdoc'.

I downloaded the source of PhpDocumentor directly from SourceForge, and it had
phpdoc available in the folder, but I'm not sure which folder to put it in so
that it's in my path for the command line.
<!--more-->

Finally I just reset the 'data_dir' setting back to the default by using this
command:

```bash
sudo pear config-set data_dir /usr/share/php/data
```

Next I installed the package again via PEAR, just like I should have originally
done.

```bash
sudo pear upgrade PhpDocumentor
```

This worked like a charm.

``` shell
$ which phpdoc
/usr/bin/phpdoc
```

[1]: http://www.greenhughes.com/content/how-install-phpdocumentor-ubuntu
