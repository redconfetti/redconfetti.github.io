---
layout: post
status: publish
published: true
title: Application Builders
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1554
wordpress_url: http://www.rubycoloredglasses.com/?p=1554
date: '2013-06-05 07:11:12 -0700'
date_gmt: '2013-06-05 07:11:12 -0700'
categories:
- Ruby on Rails
tags:
- builder
comments: []
---
<p>Everytime I setup a new Rails application I have to go through the configuration and change many things. It's as if there is a specific configuration that I prefer. For instance, I like using the Twitter Bootstrap framework for my front-end...at least just to get started. I like to use Rspec and Cucumber for testing. The list goes on.</p>
<p>I just stumbled upon this argument that the 'rails' executable provides when generating a new Rails application.</p>
<pre class="brush:shell">$ rails --help<br />
Usage:<br />
  rails new APP_PATH [options]</p>
<p>Options:<br />
  -r, [--ruby=PATH]              # Path to the Ruby binary   -b, [--builder=BUILDER]        # Path to a application builder (can be a filesystem path or URL)</pre><br />
It appears that I can configure many options in some sort of file, hosted in a Gist file under my Github account, which I can use for each new project I begin. Awesome! I'll have to explore this later, but for now here is a good article on the subject:</p>
<p><a href="http://pivotallabs.com/rails-3-application-builders/" target="_blank">Rails 3 Application Builders</a></p>
<p>I found that this article was from 2010, so documentation is likely a little better out there. Then I stumbled onto this <a href="https://github.com/RailsApps/rails-composer" target="_blank">Rails Composer</a> which asks you questions to help you setup a new Rails application. It's like a Rails generator on steroids.</p>
<p>While I was trying to follow it's instructions to run the generator, I kept getting an SSL error like so:</p>
<pre class="brush:shell">      apply  https://raw.github.com/RailsApps/rails-composer/master/composer.rb<br />
/Users/jsmith/.rvm/rubies/ruby-1.9.3-p327/lib/ruby/1.9.1/net/http.rb:799:in `connect': SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (OpenSSL::SSL::SSLError)<br />
	from /Users/jsmith/.rvm/rubies/ruby-1.9.3-p327/lib/ruby/1.9.1/net/http.rb:799:in `block in connect'<br />
	from /Users/jsmith/.rvm/rubies/ruby-1.9.3-p327/lib/ruby/1.9.1/timeout.rb:54:in `timeout'<br />
	from /Users/jsmith/.rvm/rubies/ruby-1.9.3-p327/lib/ruby/1.9.1/timeout.rb:99:in `timeout'</pre><br />
I found the recommendation to run the following to install a CURL CA bundle. After installing this I added the export command to my .zshrc file (zsh equivalent of the .bashrc file).</p>
<pre class="brush:shell">brew install curl-ca-bundle</pre><br />
After I did this, then opened a new terminal window, the generator worked just fine.</p>
<pre class="brush:shell">rails new myapp -m https://raw.github.com/RailsApps/rails-composer/master/composer.rb -T -O</pre></p>