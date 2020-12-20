---
layout: post
title: Error when building PhantomJS 2.0
date: '2015-08-08 01:34:12 -0700'
comments: true
categories:
- Uncategorized
tags:
- phantomjs
---

I was tasked with installing PhantomJS 2.0 on an Ubuntu 14.04 VPS running with
2 GB of RAM. Online discussions on
[Github](https://github.com/ariya/phantomjs/issues/13051) and
[Google Groups](https://groups.google.com/forum/#!topic/phantomjs/PAjQsrS-7ew)
seemed to have pointed to the build process requiring much RAM to complete
without error.

```shell
g++: internal compiler error: Killed (program cc1plus)

Please submit a full bug report, with preprocessed source if appropriate.
See <file:///usr/share/doc/gcc-4.8/README.Bugs> for instructions.

make[2]: *** [.obj/inspector/InspectorAllInOne.o] Error 4
make[2]: *** Waiting for unfinished jobs....
make[2]: Leaving directory `/home/app/src/phantomjs-2.0.0/src/qt/qtwebkit/Source/WebCore'
make[1]: *** [sub-Target-pri-make_first-ordered] Error 2
make[1]: Leaving directory `/home/app/src/phantomjs-2.0.0/src/qt/qtwebkit/Source/WebCore'
make: *** [sub-Source-WebCore-WebCore-pro-make_first-ordered] Error 2
```
<!--more-->

To overcome this error I checked the build.sh script and found that the script
would discover the number of CPU cores you're running on a machine and thus run
that many concurrent build processes, thus using more memory. To overcome this
you can run the script with the number of jobs specified.

``` shell
./build.sh --jobs 1
```

It turns out that this wasn't anything new. The
[official build instructions](http://phantomjs.org/build.html) actually advise
to use the `--jobs 1` argument, I just missed this because I downloaded the ZIP
file before proceeding. I did however find out that the ZIP file they have you
download still results in error, whereas pulling the source from the '2.0'
branch of Github is building much better now.
