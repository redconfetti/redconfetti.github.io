---
layout: post
status: publish
published: true
title: Issues with RVM after upgrade to OS X Mavericks
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1834
wordpress_url: http://www.rubycoloredglasses.com/?p=1834
date: '2014-10-02 21:59:00 -0700'
date_gmt: '2014-10-02 21:59:00 -0700'
categories:
- Mac OS X
- Ruby
tags: []
---
<p>So I just upgraded to OS X Mavericks (10.9.5). I also upgraded to X Code 6, and also installed the command line tools via the 'xcode-select --install' command. I also have the 'apple-gcc42' Homebrew package installed to provide GCC 4.2.</p>
<p>Still however, when I would try to install a version of Ruby via RVM, I would get this error:</p>
<pre class="brush:shell">$ rvm install ruby-1.9.3<br />
ruby-1.9.3-p547 - #removing src/ruby-1.9.3-p547 - please wait<br />
Searching for binary rubies, this might take some time.<br />
No binary rubies available for: osx/10.9/x86_64/ruby-1.9.3-p547.<br />
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.<br />
Checking requirements for osx.<br />
Certificates in '/usr/local/etc/openssl/cert.pem' are already up to date.<br />
Requirements installation successful.<br />
Installing Ruby from source to: /Users/jasonmiller/.rvm/rubies/ruby-1.9.3-p547, this may take a while depending on your cpu(s)...<br />
ruby-1.9.3-p547 - #downloading ruby-1.9.3-p547, this may take a while depending on your connection...<br />
ruby-1.9.3-p547 - #extracting ruby-1.9.3-p547 to /Users/jasonmiller/.rvm/src/ruby-1.9.3-p547 - please wait<br />
ruby-1.9.3-p547 - #applying patch /Users/jasonmiller/.rvm/patches/ruby/GH-488.patch - please wait<br />
ruby-1.9.3-p547 - #configuring - please wait<br />
Error running './configure --prefix=/Users/jasonmiller/.rvm/rubies/ruby-1.9.3-p547 --with-opt-dir=/usr/local/opt/libyaml:/usr/local/opt/readline:/usr/local/opt/libksba:/usr/local/opt/openssl --without-tcl --without-tk --disable-install-doc --enable-shared',<br />
showing last 15 lines of /Users/jasonmiller/.rvm/log/1412286075_ruby-1.9.3-p547/configure.log<br />
configure: WARNING: unrecognized options: --without-tcl, --without-tk<br />
checking build system type... i386-apple-darwin13.4.0<br />
checking host system type... i386-apple-darwin13.4.0<br />
checking target system type... i386-apple-darwin13.4.0<br />
checking whether the C compiler works... no<br />
configure: error: in `/Users/jasonmiller/.rvm/src/ruby-1.9.3-p547':<br />
configure: error: C compiler cannot create executables<br />
See `config.log' for more details<br />
There has been an error while running configure. Halting the installation.<br />
</pre><br />
I was able to get it installed and running fine by specifying for RVM to use the clang compiler.</p>
<pre class="brush:shell">$ rvm install 1.9.3 --with-gcc=clang<br />
</pre><br />
Note: Had an issue with Canvas installation, which involved some C code displayed, but it turns out that this was due to an <a href="https://issues.apache.org/jira/browse/THRIFT-2219" target="_blank">issue with the Thrift gem</a> on Mavericks.</p>
