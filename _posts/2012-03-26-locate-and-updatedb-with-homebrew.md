---
layout: post
status: publish
published: true
title: Locate and Updatedb with Homebrew
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1093
wordpress_url: http://www.redconfetti.com/?p=1093
date: '2012-03-26 14:08:59 -0700'
date_gmt: '2012-03-26 18:08:59 -0700'
categories:
- Mac OS X
tags:
- os x lion
- findutils
- homebrew
comments:
- id: 337
  author: Jamie
  author_email: jamie+nospam@stoic.net
  author_url: ''
  date: '2012-04-17 15:44:05 -0700'
  date_gmt: '2012-04-17 19:44:05 -0700'
  content: |-
    You don't even need to bother with running locate / updatedb on OS X since Spotlight is already doing all the heavy lifting to index your files. Add this to your .bashrc to simulate the locate command.

    alias locate='mdfind -name'

    The mdfind command by default will conduct a full text search of all your files, but since everything has already been indexed it's often quite a bit faster than grep. I've slowly been trying to retrain myself to use it instead of grep (but old habits die hard).

    "man mdfind" for more info
- id: 9623
  author: Hong Xu
  author_email: hong@topbug.net
  author_url: http://www.topbug.net
  date: '2013-05-06 08:16:14 -0700'
  date_gmt: '2013-05-06 08:16:14 -0700'
  content: "The solution is really simple:\r\n\r\nLC_ALL='C' sudo updatedb"
- id: 18559
  author: Dan Pritts
  author_email: danpritts@yahoo.com
  author_url: ''
  date: '2013-11-06 16:15:36 -0800'
  date_gmt: '2013-11-06 16:15:36 -0800'
  content: "unfortunately, spotlight doesn't index everything.  \r\n\r\n# mdfind -name
    krb5.conf\r\n/usr/share/man/man5/krb5.conf.5\r\n\r\n# locate
    krb5.conf\r\n/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.9.sdk/usr/share/man/man5/krb5.conf.5\r\n/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator7.0.sdk/usr/share/man/man5/krb5.conf.5\r\n/usr/share/man/man5/krb5.conf.5"
- id: 22756
  author: Valerie Parham-Thompson
  author_email: valerie.parham.thompson@gmail.com
  author_url: ''
  date: '2014-01-02 13:59:46 -0800'
  date_gmt: '2014-01-02 13:59:46 -0800'
  content: I was getting errors, too, and the solution was to create the ~/tmp.
    Thanks for the tip!
---
<p><strong><span style="color: #ff0000;">UPDATE:</span> I ran into errors and decided to not use the findutils provided by Homebrew. I simply setup the following alias in .bash_profile and this did the trick. This is using the built in locate database provided with Mac OS X Snow Leopard.</strong></p>
<pre class="brush:shell">
alias updatedb='sudo /usr/libexec/locate.updatedb'<br />
</pre></p>
<hr />
<p>I used to use MacPorts to ensure that my command line environment on my Mac was almost exclusively using MacPort provided binaries, not the built in binaries and libraries that are packaged with Mac OS X.</p>
<p>I had heard of Homebrew, but MacPorts seemed to work fine for me. Then I realized that Homebrew does the same thing, but installs software in /usr/local, which doesn't require sudo. The benefits of Homebrew seem to be <a href="http://tedwise.com/2010/08/28/homebrew-vs-macports/" target="_blank">simplicity, lack of intrusiveness, and speed</a>. I'm likely going to use it as the package manager in my Rails developer toolkit in the future.</p>
<p>I just noticed that I am not able to use the 'locate' command to search for certain matching filenames on my system. I love using this option piped into grep to find what I'm looking for, such as the path to a particular gem I'm needing to inspect the code for.</p>
<p>I installed the 'findutils' package that includes 'locate' via Homebrew.</p>
<pre class="brush:shell">$ brew install findutils<br />
==> Downloading http://ftpmirror.gnu.org/findutils/findutils-4.4.2.tar.gz<br />
######################################################################## 100.0%<br />
==> ./configure --prefix=/usr/local/Cellar/findutils/4.4.2 --program-prefix=g --disable-debug<br />
==> make install<br />
Warning: Non-libraries were installed to "lib".<br />
Installing non-libraries to "lib" is bad practice.<br />
The offending files are:<br />
/usr/local/Cellar/findutils/4.4.2/lib/charset.alias<br />
==> Summary<br />
/usr/local/Cellar/findutils/4.4.2: 19 files, 1.2M, built in 68 seconds</pre><br />
I thought that the warning regarding non-libraries being installed to "lib" stopped the package from being installed properly, but it turns out that the executables were installed with symlinks placed in /usr/local/bin (which is in my path) and pointing to the actual installed binaries. Instead of 'locate' and 'updatedb', the commands are 'glocate' and 'gupdatedb'.</p>
<p>As advised by <a href="http://superuser.com/questions/109590/whats-the-equivalent-of-linuxs-updatedb-command-for-the-mac" target="_blank">Grogs</a>, I updated my .bash_profile file to set the LOCATE_PATH to point to the database in my local users tmp directory.</p>
<p>I didn't want to have to setup a running cron daemon on my Mac, and I'm just fine with running the updatedb command manually when needed, so I simply added aliases to build and locate using the proper executable filenames.</p>
<pre class="brush:shell">alias updatedb="gupdatedb --localpaths='/Users/jmiller' --output='/Users/jmiller/tmp/locatedb'"<br />
alias locate="glocate"<br />
export LOCATE_PATH="~/tmp/locatedb"</pre><br />
Before this would work though, I did need to create a 'tmp' folder in my home directory.</p>
<pre class="brush:shell">mkdir ~/tmp</pre><br />
<span style="color: #ff0000;"><strong>UPDATE:</strong></span></p>
<p>This wasn't successful however. I started to get an error:</p>
<pre class="brush:shell">$ updatedb<br />
/usr/bin/sort: string comparison failed: Illegal byte sequence<br />
/usr/bin/sort: Set LC_ALL='C' to work around the problem.<br />
/usr/bin/sort: The strings compared were `/USERS/JMILLER/LIBRARY/APPLICATION SUPPORT/TEXTMATE/BUNDLES/SCSS.TMBUNDLE/COMMANDS/INCREASE NUMBER.TMCOMMAND' and `/USERS/JMILLER/LIBRARY/APPLICATION SUPPORT/TEXTMATE/BUNDLES/SCSS.TMBUNDLE/COMMANDS/INSERT COLOR302200246.TMCOMMAND'.</pre><br />
I'm going to just use the locate/updatedb options which come packaged with Mac OS X.</p>