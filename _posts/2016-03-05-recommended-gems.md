---
layout: post
title: Recommended Gems
categories:
- Ruby
tags:
- gems
- ruby
---

Here are some Gems we recommend that you checkout.
<!--more-->
* Authentication / Authorization
  * [devise](https://github.com/plataformatec/devise) - Flexible authentication
    solution for Rails with Warden
  * [cancan](https://github.com/ryanb/cancan) - authorization library for Rails
    which restricts what resources a given user is allowed to access
  * [pundit](https://github.com/elabs/pundit) - Minimal authorization through OO
    design and pure Ruby classes
* HTTP Clients
  * [curb](https://github.com/taf2/curb) - Ruby bindings for libcurl
  * [httparty](https://github.com/jnunemaker/httparty) - Fun HTTP client for Ruby
  * [typhoeus](https://github.com/dbalatero/typhoeus) - Multithreaded library
    for accessing web services in Ruby
* Mac OSX
  * [lunchy](https://github.com/eddiezane/lunchy) - A friendly wrapper for
    launchctl, used to manage services. Great for managing daemons installed via
    [Homebrew](http://brew.sh/), such as Postgres, Memcached, Redis, etc.
* Logging
  * [itslog](https://github.com/johnnytommy/itslog) - log formatter with color
    support
* Rails Console
  * [pry](https://github.com/pry/pry) - IRB alternative and runtime developer
    console
    * [pry-rails](https://github.com/rweng/pry-rails) - Rails initializer for
      Rails
    * [pry-remote](https://github.com/Mon-Ouie/pry-remote) - Connect to pry
      console remotely
    * [pry-nav](https://github.com/nixme/pry-nav) - Binding navigation commands
      for Pry to make a simple debugger
  * [rbenv](https://github.com/sstephenson/rbenv) - Simple Ruby version
    management. Less intrusive than RVM.
* ORM
  * [audited](https://github.com/collectiveidea/audited) - ORM extension that
    logs all changes to your Rails models
* Form Helpers
  * [simpleform](ttps://github.com/plataformatec/simple_form) - Alternative form
    helpers, tied to a simple DSL, with no opinion on markup, with support for
    Twitter Bootstrap
* Configuration
  * [figaro](https://github.com/laserlemon/figaro) - Facilitates storing
    sensitive configuration information for your app in a file not checked into
    the repository, such as AWS keys, passwords, etc.
* Testing / Continuous Integration
  * [rspec](http://rspec.info/) - Alternative to Test::Unit. Easier to read.
    More specific test support (doesn't couple view/controller testing)
  * [webmock](https://github.com/bblimke/webmock) - Library for stubbing and
    setting expectations on HTTP requests in Ruby
  * [simplecov](https://github.com/colszowka/simplecov) - SimpleCov is a
    [Code coverage](http://en.wikipedia.org/wiki/Code_coverage) analysis tool
    for Ruby 1.9+.
  * [fabrication](http://www.fabricationgem.org/) - alternative to FactoryBot
    for fixtures replacement. [Github](https://github.com/paulelliott/fabrication)
  * [database_cleaner](https://github.com/bmabey/database_cleaner) - Strategies
    for cleaning databases in Ruby. Can be used to ensure a clean state for
    testing
  * [heckle](https://github.com/seattlerb/heckle) - mutation tester that
    modifies your code and runs your tests to make sure they fail (like they
    should)
  * [cucumber-rails](https://github.com/cucumber/cucumber-rails) - Rails
    Generators for Cucumber with special support for Capybara and
    DatabaseCleaner
  * [launchy](https://github.com/copiousfreetime/launchy) - launches external
    application from within ruby programs. Used to view state of virtual page
    renders with Cucumber/Capybara
  * [capybara](https://github.com/jnicklas/capybara) - Acceptance test framework
    for web applications
  * [capybara-webkit](https://github.com/thoughtbot/capybara-webkit) - A
    capybara driver that uses WebKit via QtWebKit
  * [poltergeist](https://github.com/jonleighton/poltergeist) - Headless
    Javascript engine driver for Capybara
  * [headless](https://github.com/leonid-shevtsov/headless) - Ruby wrapper for
    Xvfb, the virtual framebuffer
  * [guard](https://github.com/guard/guard) - command line tool to easily handle
    events on file system modifications. Use 'gem search -r ^guard-' to view the
    many plugins that work with guard to automate testing, asset building, etc.
    * [guard-rspec](https://github.com/guard/guard-rspec) - automatically run
      your specs after modifying models/spec files
    * [guard-pow](https://github.com/guard/guard-pow) - automatically manage Pow
      applications restart
    * [guard-cucumber](https://github.com/guard/guard-cucumber) - automatically
      run your features
* Development
  * [capistrano](https://github.com/capistrano/capistrano) - Used to deploy
    Rails applications to hosting environments such as a VPS
  * [capistrano-unicorn](https://github.com/sosedoff/capistrano-unicorn) -
    Capistrano integration for Unicorn
  * [jasminerice](https://github.com/bradphelan/jasminerice) - Pain free
    coffeescript testing under Rails 3.1
