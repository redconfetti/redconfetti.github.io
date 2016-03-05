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
You may be used to using 'bundle install' or 'bundle exec' often, but here are some commands you might have forgotten about or never heard of.

## Bundle Init

You don't have to create your own Gemfile manually for new Ruby based projects. Bundle Init creates a new one for you.

``` shell
$ bundle init

Writing new Gemfile to /Users/myuser/Projects/hey_guys/Gemfile

$ ls -la Gemfile

-rw-r--r--  1 myuser  mygroup  64 Aug 29 17:40 Gemfile```

<h1>Bundle Console</h1>

For purer Ruby projects, this is useful. Start an IRB session in the context of the current bundle.

``` shell
$ bundle console

Resolving dependencies...

1.9.3p448 :001 > require 'rake'

 => true

1.9.3p448 :002 > Rake

 => Rake```

<h1>Bundle Open</h1>

After you have configured your <a href="http://www.rubycoloredglasses.com/2012/11/changing-the-default-text-editor/" target="_blank">default text editor</a>, which could be Vim, Emacs, Textmate, or Sublime, you can use 'bundle open' to quickly open your editor with the root directory for the gems source code loaded.

``` shell
$ bundle open rake

Resolving dependencies...```

<h1>Bundle Gem</h1>

Bundler can even help you get started with the development of a new gem.

``` shell
$ bundle gem smash_pumpkin

      create  smash_pumpkin/Gemfile

      create  smash_pumpkin/Rakefile

      create  smash_pumpkin/LICENSE.txt

      create  smash_pumpkin/README.md

      create  smash_pumpkin/.gitignore

      create  smash_pumpkin/smash_pumpkin.gemspec

      create  smash_pumpkin/lib/smash_pumpkin.rb

      create  smash_pumpkin/lib/smash_pumpkin/version.rb

Initializating git repo in /Users/redconfetti/Sites/annotate_gemfile/smash_pumpkin```

<h1>Bundle Inject</h1>

Bundle Inject is an undocumented feature added on <a href="https://github.com/bundler/bundler/blob/master/CHANGELOG.md#130pre-nov-29-2012" target="_blank">November 29, 2012, in version 1.3.0.pre</a>, implemented by Engine Yard likely for their own automation. This command allows you to add gems to your Gemfile from the command line. <a href="http://www.youtube.com/watch?v=8C4ayBHTES0" target="_blank">Great jorb Engine Yard</a>!

``` shell
$ bundle inject

bundle inject requires at least 2 arguments: "bundle inject GEM VERSION ...".```

Bundler defines this command, as well as the others, in <a href="https://github.com/bundler/bundler/blob/master/lib/bundler/cli.rb#L807" target="_blank">Bundler::CLI</a>. This file defines the command line interface for bundle commands (bundle install, bundle update, bundle exec, bundle gem,etc) using <a href="https://github.com/erikhuda/thor#description" target="_blank">Thor</a>. Thor command support standard command line style <a href="http://whatisthor.com/#options-and-flags" target="_blank">options and flags</a>.

<span style="line-height: 21px;">Here is an example of the default intended use of the command.</span>

``` shell
$ bundle inject poltergeist 1.3.0

Fetching gem metadata from https://rubygems.org/......

Fetching gem metadata from https://rubygems.org/..

Resolving dependencies...

Added to Gemfile:

  poltergeist (= 1.3.0)```

The resulting definition added to my Gemfile was very descriptive.

<pre class="brush:ruby"># Added at 2013-08-29 17:54:59 -0700 by my_user_name:

gem 'poltergeist', '= 1.3.0'```

I explored the code and it doesn't appear that there are options to include special arguments such as the branch or git repository. It does however support multiple gem argument sets like so:

``` shell
$ bundle inject poltergeist 1.3.0 pry 0.9.12.2

Fetching gem metadata from https://rubygems.org/......

Fetching gem metadata from https://rubygems.org/..

Resolving dependencies...

Added to Gemfile:

  poltergeist (= 1.3.0)

  pry (= 0.9.12.2)```

The result being:

<pre class="brush:ruby"># Added at 2013-08-29 18:09:41 -0700 by redconfetti:

gem 'poltergeist', '= 1.3.0'

gem 'pry', '= 0.9.12.2'```

 

