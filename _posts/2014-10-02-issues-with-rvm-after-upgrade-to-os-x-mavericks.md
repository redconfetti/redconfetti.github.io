---
layout: post
title: Issues with RVM after upgrade to OS X Mavericks
categories:
- Mac OS X
- Ruby
tags: []
---

So I just upgraded to OS X Mavericks (10.9.5). I also upgraded to X Code 6, and
also installed the command line tools via the `xcode-select --install` command.
I also have the 'apple-gcc42' Homebrew package installed to provide GCC 4.2.

Still however, when I would try to install a version of Ruby via RVM, I would
get this error:
<!--more-->

``` shell
$ rvm install ruby-1.9.3

ruby-1.9.3-p547 - #removing src/ruby-1.9.3-p547 - please wait
Searching for binary rubies, this might take some time.
No binary rubies available for: osx/10.9/x86_64/ruby-1.9.3-p547.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for osx.
Certificates in '/usr/local/etc/openssl/cert.pem' are already up to date.
Requirements installation successful.
Installing Ruby from source to: /Users/jasonmiller/.rvm/rubies/ruby-1.9.3-p547, this may take a while depending on your cpu(s)...
ruby-1.9.3-p547 - #downloading ruby-1.9.3-p547, this may take a while depending on your connection...
ruby-1.9.3-p547 - #extracting ruby-1.9.3-p547 to /Users/jasonmiller/.rvm/src/ruby-1.9.3-p547 - please wait
ruby-1.9.3-p547 - #applying patch /Users/jasonmiller/.rvm/patches/ruby/GH-488.patch - please wait
ruby-1.9.3-p547 - #configuring - please wait
Error running './configure --prefix=/Users/jasonmiller/.rvm/rubies/ruby-1.9.3-p547
--with-opt-dir=/usr/local/opt/libyaml:/usr/local/opt/readline:/usr/local/opt/libksba:/usr/local/opt/openssl
--without-tcl --without-tk --disable-install-doc --enable-shared', showing last 15 lines of /Users/jasonmiller/.rvm/log/1412286075_ruby-1.9.3-p547/configure.log
configure: WARNING: unrecognized options: --without-tcl, --without-tk
checking build system type... i386-apple-darwin13.4.0
checking host system type... i386-apple-darwin13.4.0
checking target system type... i386-apple-darwin13.4.0
checking whether the C compiler works... no
configure: error: in `/Users/jasonmiller/.rvm/src/ruby-1.9.3-p547':
configure: error: C compiler cannot create executables
See `config.log' for more details
There has been an error while running configure. Halting the installation.
```

I was able to get it installed and running fine by specifying for RVM to use
the clang compiler.

```shell
rvm install 1.9.3 --with-gcc=clang
```

Note: Had an issue with Canvas installation, which involved some C code
displayed, but it turns out that this was due to an
[issue with the Thrift gem](https://issues.apache.org/jira/browse/THRIFT-2219)
on Mavericks.
