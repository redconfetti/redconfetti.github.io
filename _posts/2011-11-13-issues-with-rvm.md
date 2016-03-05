---
layout: post
status: publish
published: true
title: Issues with RVM
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 912
wordpress_url: http://www.redconfetti.com/?p=912
date: '2011-11-13 20:11:04 -0800'
date_gmt: '2011-11-14 00:11:04 -0800'
categories:
- Ruby on Rails
tags:
- rails
- os x lion
- rvm
comments: []
---
<p>I recently decided to check out <a title="BrowserCMS - BrowserCMS - Open Source, Ruby on Rails CMS" href="http://www.browsercms.org/" target="_blank">BrowserCMS</a>, to evaluate how it work and decide to use it...or <a title="Refinery CMS - a CMS that supports Rails 3" href="http://www.refinerycms.com/" target="_blank">RefineryCMS</a>. I didn't expect that BrowserCMS would require Ruby 1.9.2. I've been running with Ruby 1.8.6 or 1.8.7 for quite a while now without any issues. It looks like it was time that I install <a title="RVM: Ruby Version Manager" href="https://rvm.beginrescueend.com/" target="_blank">RVM: Ruby Version Manager</a>.</p>
<p>I read through the documentation, followed the instructions to ensure that prerequisite software was installed. I specifically made sure that certain commands that it stated would be needed were all in my command line path under /opt/local, where the MacPorts all install. I try to maintain a command line environment that's almost entirely dependent on MacPorts. Prior to ensuring this I ran into issues where software I've tried to install was using one utility or library provided natively by Mac OS X, while using some other utility or library I've installed separately, conflict due to differences and stop certain software from building properly upon install.</p>
<p>I also took note that since I'm using MacPorts, I should <a href="https://rvm.beginrescueend.com/integration/macports/" target="_blank">configure my $HOME/.rvmrc</a> file so that RVM uses my MacPort libraries when building gems.</p>
<h2>Installation of Ruby</h2><br />
The installation of the RVM software itself went smoothly. I felt good knowing that I was about to learn how RVM works and it will become another tool that I gladly assign to my arsenal of software to be used in the future (and add to my resume).</p>
<p>Then I ran into an issue with something in 'readline.c' when I attempted to install Ruby 1.8.7.</p>
<pre class="brush:rails">jason@imac ~$ rvm install 1.8.7 --with-openssl-dir=/opt/local</p>
<p>Installing Ruby from source to: /Users/jason/.rvm/rubies/ruby-1.8.7-p352, this may take a while depending on your cpu(s)...</p>
<p>ruby-1.8.7-p352 - #fetching<br />
ruby-1.8.7-p352 - #extracting ruby-1.8.7-p352 to /Users/jason/.rvm/src/ruby-1.8.7-p352<br />
ruby-1.8.7-p352 - #extracted to /Users/jason/.rvm/src/ruby-1.8.7-p352<br />
Applying patch 'stdout-rouge-fix' (located at /Users/jason/.rvm/patches/ruby/1.8.7/stdout-rouge-fix.patch)<br />
ruby-1.8.7-p352 - #configuring<br />
ruby-1.8.7-p352 - #compiling<br />
ERROR: Error running 'make ', please read /Users/jason/.rvm/log/ruby-1.8.7-p352/make.log<br />
ERROR: There has been an error while running make. Halting the installation.</p>
<p>jason@imac ~$ tail -10 /Users/jason/.rvm/log/ruby-1.8.7-p352/make.log<br />
.<br />
.<br />
.<br />
compiling readline<br />
/usr/bin/gcc-4.2 -I. -I../.. -I../../. -I../.././ext/readline -DHAVE_READLINE_READLINE_H -DHAVE_READLINE_HISTORY_H -DHAVE_RL_FILENAME_COMPLETION_FUNCTION -DHAVE_RL_COMPLETION_MATCHES -DHAVE_RL_DEPREP_TERM_FUNCTION -DHAVE_RL_COMPLETION_APPEND_CHARACTER -DHAVE_RL_BASIC_WORD_BREAK_CHARACTERS -DHAVE_RL_COMPLETER_WORD_BREAK_CHARACTERS -DHAVE_RL_BASIC_QUOTE_CHARACTERS -DHAVE_RL_COMPLETER_QUOTE_CHARACTERS -DHAVE_RL_FILENAME_QUOTE_CHARACTERS -DHAVE_RL_ATTEMPTED_COMPLETION_OVER -DHAVE_RL_LIBRARY_VERSION -DHAVE_RL_EVENT_HOOK -DHAVE_RL_CLEANUP_AFTER_SIGNAL -DHAVE_REPLACE_HISTORY_ENTRY -DHAVE_REMOVE_HISTORY -I/opt/local/include -D_XOPEN_SOURCE -D_DARWIN_C_SOURCE  -I/opt/local/include -fno-common -arch x86_64 -g -Os -pipe -no-cpp-precomp  -fno-common -pipe -fno-common   -c readline.c<br />
readline.c: In function &lsquo;username_completion_proc_call&rsquo;:<br />
readline.c:730: error: &lsquo;username_completion_function&rsquo; undeclared (first use in this function)<br />
readline.c:730: error: (Each undeclared identifier is reported only once<br />
readline.c:730: error: for each function it appears in.)<br />
make[1]: *** [readline.o] Error 1<br />
make: *** [all] Error 1</pre><br />
I tried to install 'readline' using MacPorts because I read online that this was a common issue with Mac OS X users under Snow Leopard. This didn't help, and in fact I got an error after trying to install readline from source as recommended in the forum post I found regarding the subject.</p>
<p>After further search, I realized that the RVM documentation (which is pretty good I must say) has <a title="RVM install ruby readline" href="https://rvm.beginrescueend.com/packages/readline/" target="_blank">a page devoted to this issue</a>.</p>
<h2>Issue Installing Custom Gems</h2><br />
Later I had installed Ruby 1.9.2 as well, because I needed it for BrowserCMS. I was trying to install gems for Ruby, but each time it would give me the 'Invalid gemspec ... invalid date format in specification' error that I recall from a time when I was using a newer version of RubyGems than the gems I had installed were made for, like:</p>
<pre class="brush:rails">Invalid gemspec in [/opt/local/lib/ruby/gems/1.8/specifications/paperclip-2.4.3.gemspec]: invalid date format in specification: "2011-10-05 00:00:00.000000000Z"</pre><br />
I made sure I was running under Ruby 1.9.2, and that the version of RubyGems would be the one associated with Ruby version 1.9.2, and then upgraded it to version</p>
<pre class="brush:rails">imac:Sites jason$ rvm 1.9.2<br />
imac:Sites jason$ rvm gemdir<br />
/Users/jason/.rvm/gems/ruby-1.9.2-p290<br />
imac:Sites jason$ rvm rubygems current<br />
Removing old Rubygems files...<br />
Installing rubygems-1.8.10 for ruby-1.9.2-p290 ...<br />
Installation of rubygems completed successfully.</pre><br />
I tried to check or change which version of gem I was using, and it would tell me it's using one installed under RVM, but still it would give errors as if it was using an older version of RubyGems.</p>
<pre class="brush:rails">imac:Sites jason$ which gem<br />
/Users/jason/.rvm/rubies/ruby-1.9.2-p290/bin/gem</p>
<p>imac:Sites jason$ sudo gem install browsercms</p>
<p>Invalid gemspec in [/opt/local/lib/ruby/gems/1.8/specifications/capistrano-2.9.0.gemspec]: invalid date format in specification: "2011-09-24 00:00:00.000000000Z"</pre><br />
I then found out that I had a .gemrc file setup that was loading in my command line environment, and thus interfering with the dependency loading which RVM provides.</p>
<pre class="brush:rails">imac:Sites jason$ cd ~<br />
imac:~ jason$ cat .gemrc<br />
gemhome: /opt/local/lib/ruby/gems/1.8<br />
gempath:<br />
 - /opt/local/lib/ruby/gems/1.8<br />
 - /Users/jason/gems<br />
imac:~ jason$</pre><br />
I simply renamed this file as .gemrc.bak and this resolved the issue so that gems I install under Ruby v1.9.2 will use the RubyGem version associated with the Ruby 1.9.2 installation.</p>
