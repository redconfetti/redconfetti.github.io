---
layout: post
title: Application Builders
date: '2013-06-05 07:11:12 -0700'
categories:
- Ruby on Rails
tags:
- builder
comments: []
---
Everytime I setup a new Rails application I have to go through the configuration and change many things. It's as if there is a specific configuration that I prefer. For instance, I like using the Twitter Bootstrap framework for my front-end...at least just to get started. I like to use Rspec and Cucumber for testing. The list goes on.

I just stumbled upon this argument that the 'rails' executable provides when generating a new Rails application.

``` shell
$ rails --help

Usage:

  rails new APP_PATH [options]

Options:

  -r, [--ruby=PATH]              # Path to the Ruby binary   -b, [--builder=BUILDER]        # Path to a application builder (can be a filesystem path or URL)```

It appears that I can configure many options in some sort of file, hosted in a Gist file under my Github account, which I can use for each new project I begin. Awesome! I'll have to explore this later, but for now here is a good article on the subject:

<a href="http://pivotallabs.com/rails-3-application-builders/" target="_blank">Rails 3 Application Builders</a>

I found that this article was from 2010, so documentation is likely a little better out there. Then I stumbled onto this <a href="https://github.com/RailsApps/rails-composer" target="_blank">Rails Composer</a> which asks you questions to help you setup a new Rails application. It's like a Rails generator on steroids.

While I was trying to follow it's instructions to run the generator, I kept getting an SSL error like so:

``` shell
      apply  https://raw.github.com/RailsApps/rails-composer/master/composer.rb

/Users/jsmith/.rvm/rubies/ruby-1.9.3-p327/lib/ruby/1.9.1/net/http.rb:799:in `connect': SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (OpenSSL::SSL::SSLError)

    from /Users/jsmith/.rvm/rubies/ruby-1.9.3-p327/lib/ruby/1.9.1/net/http.rb:799:in `block in connect'

    from /Users/jsmith/.rvm/rubies/ruby-1.9.3-p327/lib/ruby/1.9.1/timeout.rb:54:in `timeout'

    from /Users/jsmith/.rvm/rubies/ruby-1.9.3-p327/lib/ruby/1.9.1/timeout.rb:99:in `timeout'```

I found the recommendation to run the following to install a CURL CA bundle. After installing this I added the export command to my .zshrc file (zsh equivalent of the .bashrc file).

``` shell
brew install curl-ca-bundle```

After I did this, then opened a new terminal window, the generator worked just fine.

``` shell
rails new myapp -m https://raw.github.com/RailsApps/rails-composer/master/composer.rb -T -O```

