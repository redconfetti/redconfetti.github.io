---
layout: post
title: Generators Not Working in Rails 2.3.8
date: '2012-05-16 01:14:15 -0700'
categories:
- Gems
tags: []
comments: []
---
<p>I'm currently working on a gem that is going to use a generator to create files in a Rails 2.3.8 application. One of the applications we're still using is using Rails 2.3.8, so I have to make a gem compatible with that version of Rails.</p>
<p>I installed Bundler v1.0.22 and configured it to work with the Rails app, and then followed <a href="http://blog.wyeworks.com/2010/9/23/creating-your-own-generators-on-rails-2-3" target="_blank">many</a> <a href="http://www.allenwei.cn/the-missing-guide-of-rails-2-generator-part-1/" target="_blank">instructions</a> and various configurations to get my generator classes to load. Every time I would try to run the generator however it simply gave me the error "Couldn't find 'hello' generator".</p>
<p>I eventually found out that <a href="https://github.com/carlhuda/bundler/issues/210" target="_blank">gems loaded via Git or Path via Bundler fail to load the generators</a>. I was able to confirm this by loading the Devise gem via Git URI, and it's generators failed to load. As I'm currently developing a gem, I need to be able to load it in dynamically via Git or via direct path...so this sucks.</p>
<p>I found that I could use the 'gem server' command to run a local gem server, so I've been using the following commands to package up my gem and install it to the default system RubyGems (I'm using RVM), after I've updated the version for the gem in the gemspec (or version.rb file) in the gem source directory.</p>
<pre class="brush:shell">
$ gem build ~/Documents/gems/my-gem/my-gem.gemspec<br />
$ gem install --local my-gem-0.0.2.gem<br />
Successfully installed my-gem-0.0.2<br />
1 gem installed<br />
Installing ri documentation for my-gem-0.0.2...<br />
Building YARD (yri) index for my-gem-0.0.2...<br />
Installing RDoc documentation for my-gem-0.0.2...<br />
</pre></p>
<p>I then go into the Gemfile for my Rails 2.3.8 app and update the version, each time like so. As you can see I have a 'source' command for Bundler to obtain gems from my locally running Gem server.</p>
<pre class="brush:rails">
source "http://0.0.0.0:8808/"</p>
<p>gem 'my-gem', '0.0.2'<br />
</pre></p>
<p>I run a 'bundle install', then run 'bundle exec ruby script/generate' to see if the generator defined in my gem is registering with the Rails app. Unfortunately so far the Devise generators are registering, but mine are not. I'm still investigating.</p>
<p>To remove the previous un-successful gems, I use this command.</p>
<pre class="brush:shell">
$ gem uninstall my-gem -v 0.0.1<br />
Successfully uninstalled my-gem-0.0.1<br />
</pre></p>
<p>Further investigation just revealed that somehow my gems are being installed, but are empty (no files). It's obvious now why my generators aren't registering, they aren't even declared in a file. I found that I needed to make the commits to my Git repository for my gem, and push the changes, and then run the 'gem build' command from within the gem directory itself. </p>
<p>Prior to this it got these errors:</p>
<pre class="brush:shell">
$ gem build ~/Documents/gems/my-gem/my-gem.gemspec<br />
fatal: Not a git repository (or any of the parent directories): .git<br />
fatal: Not a git repository (or any of the parent directories): .git<br />
fatal: Not a git repository (or any of the parent directories): .git<br />
WARNING:  no homepage specified<br />
  Successfully built RubyGem<br />
  Name: my-gem<br />
  Version: 0.0.3<br />
  File: kabam-gmo-api-rails2-0.0.8.gem<br />
</pre></p>
<p>If I run 'gem build my-gem.gemspec' from inside of the gem folder, these errors did not occur. I simply rebuilt my gem, and another gem I also developed which it depends on , installed them in the default system gem set (which makes them available via my gem server). Then I ran 'bundle install' on my Rails 2 app, then 'bundle exec ruby script/generate' and now I see my generator.</p>
<p>[UPDATE] Sept 11, 2012:</p>
<p>I'm finally deploying my app, which relies on this gem I've developed with a Rails 2 generator. We don't yet have a gem server setup, so I'll have to rely on the application loading the gem via Bundler from the Git repository. As noted above, the generators I've included aren't listed as available when I run 'bundle exec script/generate', nor are they executable when trying to run 'bundle exec script/generate kabam_gmo_api_install'</p>
<p>Further investigation shows that gems loaded from a gem server are placed in an accessible path, however gems originating locally or from git are loaded under a bundler gems path.</p>
<pre class="brush:shell">
$ bundle list kabam-gmo-api-rails2<br />
/Users/jsmith/.rvm/gems/ruby-1.8.7-p358@myapp/bundler/gems/gmo-api-rails2-7296b5229c7c</p>
<p>$ bundle list rspec<br />
/Users/jsmith/.rvm/gems/ruby-1.8.7-p358@myapp/gems/rspec-1.3.2<br />
</pre></p>
<p>I suspect that this alternative gem path just isn't present in the environment when bundler installs via Git or local.</p>
<p>I found an article by Yehuda Katz that <a href="http://yehudakatz.com/2010/04/12/some-of-the-problems-bundler-solves/" target="_blank">explains the internals of Bundler</a> further. This issue just isn't worth the time and effort to resolve. </p>
<p>I've just included instructions for building the gems from source, installing to Gem path, running 'bundle install' with just the install gem names, running generators, restoring the Gemfile configuration to use Git repo URI's, and running 'bundle install' again.</p>
