---
layout: post
title: Establishing New Ruby Environment in a Folder using RVM
date: '2012-05-15 17:58:16 -0700'
categories:
- Ruby on Rails
tags:
- rvm
comments: []
---
<p>I know this is documented on the <a href="https://rvm.io/workflow/rvmrc/" target="_blank">official RVM website</a>, but I hate having to look it up over and over again each time I want to create a new RVMRC file.</p>
<pre class="brush:shell">
$ mkdir -p ~/projects/rails2test<br />
$ cd ~/projects/rails2test<br />
$ rvm --rvmrc --create 1.8.7@rails2test<br />
$ cd ..<br />
$ cd rails2test<br />
==============================================================================<br />
= NOTICE                                                                     =<br />
==============================================================================<br />
= RVM has encountered a new or modified .rvmrc file in the current directory =<br />
= This is a shell script and therefore may contain any shell commands.       =<br />
=                                                                            =<br />
= Examine the contents of this file carefully to be sure the contents are    =<br />
= safe before trusting it! ( Choose v[iew] below to view the contents )      =<br />
==============================================================================<br />
Do you wish to trust this .rvmrc file? (/Users/jmiller/Documents/rails2-apps/.rvmrc)<br />
y[es], n[o], v[iew], c[ancel]> y<br />
</pre></p>
<p>I'm needing to setup a Rails 2.3.8 system, so I can test my gem for compatibility between it and Rails 3.0.9.</p>
<p>I stumbled onto <a href="http://ecmanaut.blogspot.com/2011/09/running-old-rails-238-with-rvm.html" target="_blank">an article</a> with suggestions for how to install RubyGems for Rails 2.3.8. This seems to run with errors, but the last command seemed to complete without errors other than 'README' not found:</p>
<pre class="brush:shell">
$ rvm all do gem install -v 1.4.2 rubygems-update</p>
<p>$ rvm gem update --system 1.4.2</p>
<p>$ update_rubygems</p>
<p>$ rvm all do gem install -v 2.3.8 rails</p>
<p>$ rails --version<br />
Rails 2.3.8<br />
$ ruby --version<br />
ruby 1.8.7 (2012-02-08 patchlevel 358) [i686-darwin10.8.0]<br />
</pre></p>
