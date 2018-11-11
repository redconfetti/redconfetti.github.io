---
layout: post
title: C Development
categories:
- Linux
- C Programming
tags:
- make
- gcc
---

I learned C in a high school programming class, developing basic command line
programs within Borland's Turbo C IDE. It wasn't until several years later that
I'd discover how to apply my programming skills to a currently relevant
computing interface (the web) when I discovered PHP. This eventually led to the
exploration of Linux / Unix.

If you've ever worked with Linux distributions you will have discovered that
there are essentially two approaches to installing software:

* Via a Package Manager (RPM, Yum, Apt-Get)
* From source code

The source code approach consists of the following steps:

* Downloading the source in a tarball (tar.gz) or ZIP file
* Unpackaging the source into a directory
* Running the configure script (`./configure`)

For instance, to install the `nano` text editor from source, I simply did the
following from my Ubuntu command line.

```
$ wget -q https://www.nano-editor.org/dist/v2.6/nano-2.6.1.tar.gz
$ tar -zxvf nano-2.6.1.tar.gz
$ cd nano-2.6.1
$ ./configure
$ make
$ make install
```

In a nutshell this process involves a scan of your system to look for available
tools that can be used to build the software that you're wishing to install. It
is possible to run the configure script with certain flags/parameters to modify
the features included in the program, or specify alternative libraries or paths
that the program should use when installed via `make install`.

More about this process is explained in
[The Magic Behind Configure, Make, Make Install](https://robots.thoughtbot.com/the-magic-behind-configure-make-make-install)


