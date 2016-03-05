---
layout: post
title: Error when building PhantomJS 2.0
date: '2015-08-08 01:34:12 -0700'
categories:
- Uncategorized
tags:
- phantomjs
---
<p>I was tasked with installing PhantomJS 2.0 on an Ubuntu 14.04 VPS running with 2 GB of RAM. Online discussions on <a href="https://github.com/ariya/phantomjs/issues/13051" target="_blank">Github</a> and <a href="https://groups.google.com/forum/#!topic/phantomjs/PAjQsrS-7ew" target="_blank">Google Groups</a> seemed to have pointed to the build process requiring much RAM to complete without error.</p>
<pre class="brush:shell">g++: internal compiler error: Killed (program cc1plus)<br />
Please submit a full bug report,<br />
with preprocessed source if appropriate.<br />
See <file:///usr/share/doc/gcc-4.8/README.Bugs> for instructions.<br />
make[2]: *** [.obj/inspector/InspectorAllInOne.o] Error 4<br />
make[2]: *** Waiting for unfinished jobs....<br />
make[2]: Leaving directory `/home/app/src/phantomjs-2.0.0/src/qt/qtwebkit/Source/WebCore'<br />
make[1]: *** [sub-Target-pri-make_first-ordered] Error 2<br />
make[1]: Leaving directory `/home/app/src/phantomjs-2.0.0/src/qt/qtwebkit/Source/WebCore'<br />
make: *** [sub-Source-WebCore-WebCore-pro-make_first-ordered] Error 2<br />
</pre><br />
To overcome this error I checked the build.sh script and found that the script would discover the number of CPU cores you're running on a machine and thus run that many concurrent build processes, thus using more memory. To overcome this you can run the script with the number of jobs specified.</p>
<pre class="brush:shell">./build.sh --jobs 1<br />
</pre><br />
It turns out that this wasn't anything new. The <a href="http://phantomjs.org/build.html" target="_blank">official build instructions</a> actually advise to use the `--jobs 1` argument, I just missed this because I downloaded the ZIP file before proceeding. I did however find out that the ZIP file they have you download still results in error, whereas pulling the source from the '2.0' branch of Github is building much better now.</p>
