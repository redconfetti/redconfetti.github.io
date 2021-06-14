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

You may be used to using 'bundle install' or 'bundle exec' often, but here are
some commands you might have forgotten about or never heard of.

## Bundle Init

You don't have to create your own Gemfile manually for new Ruby based projects.
Bundle Init creates a new one for you.

``` shell
$ bundle init
Writing new Gemfile to /Users/myuser/Projects/hey_guys/Gemfile
$ ls -la Gemfile
-rw-r--r--  1 myuser  mygroup  64 Aug 29 17:40 Gemfile
```
<!--more-->

## Bundle Console

For purer Ruby projects, this is useful. Start an IRB session in the context of
the current bundle.

``` shell
$ bundle console
Resolving dependencies...
1.9.3p448 :001 > require 'rake'
 => true
1.9.3p448 :002 > Rake
 => Rake
```

## Bundle Open

After you have configured your [default text editor], which could be Vim, Emacs,
Textmate, or Sublime, you can use 'bundle open' to quickly open your editor with
the root directory for the gems source code loaded.

[default text editor]: ./2012/11/changing-the-default-text-editor/

``` shell
$ bundle open rake
Resolving dependencies...
```

## Bundle Gem

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
Initializating git repo in /Users/redconfetti/Sites/annotate_gemfile/smash_pumpkin
```

## Bundle Inject

Bundle Inject is an undocumented feature added on
[November 29, 2012, in version 1.3.0.pre], implemented by Engine Yard likely for
their own automation. This command allows you to add gems to your Gemfile from
the command line. [Great jorb Engine Yard]!

[November 29, 2012, in version 1.3.0.pre]: https://github.com/bundler/bundler/blob/master/CHANGELOG.md#130pre-nov-29-2012
[Great jorb Engine Yard]: http://www.youtube.com/watch?v=8C4ayBHTES0

``` shell
$ bundle inject

bundle inject requires at least 2 arguments: "bundle inject GEM VERSION ...".
```

Bundler defines this command, as well as the others, in [Bundler::CLI]. This
file defines the command line interface for bundle commands (bundle install,
bundle update, bundle exec, bundle gem,etc) using [Thor]. Thor command support
standard command line style [options and flags].

[Bundler::CLI]: https://github.com/bundler/bundler/blob/master/lib/bundler/cli.rb#L807
[Thor]: https://github.com/erikhuda/thor#description
[options and flags]: http://whatisthor.com/#options-and-flags

Here is an example of the default intended use of the command.

``` shell
$ bundle inject poltergeist 1.3.0
Fetching gem metadata from https://rubygems.org/......
Fetching gem metadata from https://rubygems.org/..
Resolving dependencies...
Added to Gemfile:
  poltergeist (= 1.3.0)
```

The resulting definition added to my Gemfile was very descriptive.

``` ruby
# Added at 2013-08-29 17:54:59 -0700 by my_user_name:
gem 'poltergeist', '= 1.3.0'
```

I explored the code and it doesn't appear that there are options to include
special arguments such as the branch or git repository. It does however support
multiple gem argument sets like so:

``` shell
$ bundle inject poltergeist 1.3.0 pry 0.9.12.2
Fetching gem metadata from https://rubygems.org/......
Fetching gem metadata from https://rubygems.org/..
Resolving dependencies...
Added to Gemfile:
  poltergeist (= 1.3.0)
  pry (= 0.9.12.2)
```

The result being:

``` ruby
# Added at 2013-08-29 18:09:41 -0700 by redconfetti:
gem 'poltergeist', '= 1.3.0'
gem 'pry', '= 0.9.12.2'
```
