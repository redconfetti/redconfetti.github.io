---
layout: post
title: Exploring Bundler Commands
date: '2013-08-30 01:11:49 -0700'
categories:
- Gems
tags:
- rubygems
- bundler
- Thor
---
<p>You may be used to using 'bundle install' or 'bundle exec' often, but here are some commands you might have forgotten about or never heard of.</p>
<h2>Bundle Init</h2><br />
You don't have to create your own Gemfile manually for new Ruby based projects. Bundle Init creates a new one for you.</p>
<pre class="brush:shell">$ bundle init<br />
Writing new Gemfile to /Users/myuser/Projects/hey_guys/Gemfile<br />
$ ls -la Gemfile<br />
-rw-r--r--  1 myuser  mygroup  64 Aug 29 17:40 Gemfile</pre></p>
<h1>Bundle Console</h1><br />
For purer Ruby projects, this is useful. Start an IRB session in the context of the current bundle.</p>
<pre class="brush:shell">$ bundle console<br />
Resolving dependencies...<br />
1.9.3p448 :001 > require 'rake'<br />
 => true<br />
1.9.3p448 :002 > Rake<br />
 => Rake</pre></p>
<h1>Bundle Open</h1><br />
After you have configured your <a href="http://www.rubycoloredglasses.com/2012/11/changing-the-default-text-editor/" target="_blank">default text editor</a>, which could be Vim, Emacs, Textmate, or Sublime, you can use 'bundle open' to quickly open your editor with the root directory for the gems source code loaded.</p>
<pre class="brush:shell">$ bundle open rake<br />
Resolving dependencies...</pre></p>
<h1>Bundle Gem</h1><br />
Bundler can even help you get started with the development of a new gem.</p>
<pre class="brush:shell">$ bundle gem smash_pumpkin<br />
      create  smash_pumpkin/Gemfile<br />
      create  smash_pumpkin/Rakefile<br />
      create  smash_pumpkin/LICENSE.txt<br />
      create  smash_pumpkin/README.md<br />
      create  smash_pumpkin/.gitignore<br />
      create  smash_pumpkin/smash_pumpkin.gemspec<br />
      create  smash_pumpkin/lib/smash_pumpkin.rb<br />
      create  smash_pumpkin/lib/smash_pumpkin/version.rb<br />
Initializating git repo in /Users/redconfetti/Sites/annotate_gemfile/smash_pumpkin</pre></p>
<h1>Bundle Inject</h1><br />
Bundle Inject is an undocumented feature added on <a href="https://github.com/bundler/bundler/blob/master/CHANGELOG.md#130pre-nov-29-2012" target="_blank">November 29, 2012, in version 1.3.0.pre</a>, implemented by Engine Yard likely for their own automation. This command allows you to add gems to your Gemfile from the command line. <a href="http://www.youtube.com/watch?v=8C4ayBHTES0" target="_blank">Great jorb Engine Yard</a>!</p>
<pre class="brush:shell">$ bundle inject<br />
bundle inject requires at least 2 arguments: "bundle inject GEM VERSION ...".</pre><br />
Bundler defines this command, as well as the others, in <a href="https://github.com/bundler/bundler/blob/master/lib/bundler/cli.rb#L807" target="_blank">Bundler::CLI</a>. This file defines the command line interface for bundle commands (bundle install, bundle update, bundle exec, bundle gem,etc) using <a href="https://github.com/erikhuda/thor#description" target="_blank">Thor</a>. Thor command support standard command line style <a href="http://whatisthor.com/#options-and-flags" target="_blank">options and flags</a>.</p>
<p><span style="line-height: 21px;">Here is an example of the default intended use of the command.</span></p>
<pre class="brush:shell">$ bundle inject poltergeist 1.3.0<br />
Fetching gem metadata from https://rubygems.org/......<br />
Fetching gem metadata from https://rubygems.org/..<br />
Resolving dependencies...<br />
Added to Gemfile:<br />
  poltergeist (= 1.3.0)</pre><br />
The resulting definition added to my Gemfile was very descriptive.</p>
<pre class="brush:ruby"># Added at 2013-08-29 17:54:59 -0700 by my_user_name:<br />
gem 'poltergeist', '= 1.3.0'</pre><br />
I explored the code and it doesn't appear that there are options to include special arguments such as the branch or git repository. It does however support multiple gem argument sets like so:</p>
<pre class="brush:shell">$ bundle inject poltergeist 1.3.0 pry 0.9.12.2<br />
Fetching gem metadata from https://rubygems.org/......<br />
Fetching gem metadata from https://rubygems.org/..<br />
Resolving dependencies...<br />
Added to Gemfile:<br />
  poltergeist (= 1.3.0)<br />
  pry (= 0.9.12.2)</pre><br />
The result being:</p>
<pre class="brush:ruby"># Added at 2013-08-29 18:09:41 -0700 by redconfetti:<br />
gem 'poltergeist', '= 1.3.0'<br />
gem 'pry', '= 0.9.12.2'</pre><br />
 </p>
