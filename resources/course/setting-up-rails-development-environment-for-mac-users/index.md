---
layout: page
status: publish
published: true
title: Setting up a Rails Development Environment for Mac Users
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: ''
author_login: redconfetti
author_email: jason@redconfetti.com
wordpress_id: 115
wordpress_url: http://rails.redconfetti.com/?page_id=115
date: '2011-10-13 18:28:05 -0700'
comments: true
categories: []
tags: []
comments: []
---

This article provides instructions for setting up a Ruby on Rails development
environment using Mac OS X. Please see the next article for instructions on
setup for Windows or Linux.

Mac OS X because it's the most popular operating system within the Ruby on Rails
community, with many tutorials, screencasts, and other resources devoted toward
those developing for Ruby on Rails using Mac OS X.

## Text Editor

When you're working on a Ruby on Rails application it's best if you have a text
editor which provides Ruby code syntax highlighting / coloring. This makes it
easy to detect when you're making a mistake, and makes for a pleasant coding
experience (don't you like colors?!). Also important is a project folder viewer
which displays the various files in each folder of the project.

I highly recommend [TextMate]. At a price of $55, it's well worth it as it makes
your code look beautiful and easy to edit. There are alternative programs such
as [jEdit], or an entire development environment known as [Aptana Radrails],
however these are complicated and I don't recommend them to someone who is just
getting into Ruby on Rails web development.

If you're not wanting to pay for Textmate, then I can recommend [RedCar], which
runs on jRuby/Java and can be kind of slow, but closely resembles Textmate.

[RedCar]: https://github.com/redcar/redcar

If you're working on an open source project, or willing to pay $15 a month, then
you can use a web-based IDE tailored for Rails projects called
[Cloud9](http://c9.io/).

## Package Management

To work on a Ruby on Rails project, you'll need to use the command line to run
certain commands. I also recommend that you setup your computer so that you can
installation and use any Linux-based software or libraries which you may find
you need to have available at some point in the future. You may be aware that
Windows software is made to be installed and run for any version of Windows (XP,
Vista, 7), and just the same Mac software will install and run on Mac computers
running a recent Mac OS X version (such as Leopard or Lion).

Software made for
Linux is not so easy to install on any computer running a Linux operating
system. This is because Linux software is originally developed and provided as
source code, and requires a very technical understanding of how to compile and
install the software on the system. At the same time, there are not just
versions of Linux (older and newer), but many various distributions that are
being maintained and updated. Each distribution, which is put together and
distributed by an organization or sometimes by an individual, may store
programs, libraries, and configuration files in entirely different directories
inside the file system.

A Linux distribution may also come with certain systems or utilities which are
designed to work with that distributions configuration of programs, libraries,
and configuration files. Because of this, there is not a one-size fits all
installer available for Linux programs that will work with any distribution.
This is why the most popular Linux distributions come with a program known as a
Package Manager, which downloads and installs software as packages which are
intended for that specific distribution. For example there is a very popular
Linux distribution known as [Ubuntu](http://www.ubuntu.com/), which comes with
a package manager called Synaptic. When an Ubuntu user chooses to install the
Apache2 web server via Synaptic, a package is downloaded and installed which is
specifically designed to work with Ubuntu. For the purpose of this website, the
instructions below will help you to create a link to the command line/terminal
for your system, install a package manager, and then install the software needed
via that package manager. This way your version of Ruby, Apache web server, and
other libraries, will all be associated with each other, and will make for a
smoother experience without as many complications. ## Command Prompt for Mac OS
X - Terminal The command line on a Mac is known as 'Terminal' and is available
under Applications > Utilities, in the Finder window. Drag this program to your
Dock on the bottom of the screen so you can open it quickly and easily in the
future as needed. Even though Mac OS X comes packaged with Ruby, we recommend
that you install a package manager known as
[MacPorts](http://www.macports.org/index.php) and then establish all the
software installed by MacPorts as the ones used in your command line
environment.

## Installing Ruby

The first step in getting started with Ruby on Rails is to install the Ruby
interpreter itself. Many programming languages, such as C or Java, require a
program known as a compiler which converts the text coding of the language into
an executable program for use on the computer. Ruby is a scripting language that
is run by an interpreter program in realtime, like PHP or Perl, and thus it
doesn't require compiling. You simply run the command for ruby to execute the
script and it does what the script is programmed to do.

If you're running Mac OS X, you don't need to install Ruby because you already
have it. It is packaged with your operating system.

## Installing Ruby Gems

Most programming languages provide standard features such as variables,
operators, conditional statements, loops, etc. In addition to these standard
features a core set of libraries are typically provided which provide functions
for working with the file system, networking, different types of variables, etc.
All these features and libraries are the building blocks of any computer
program, however there are many common actions which you may want to perform
with your program that you would need to program from scratch. Luckily there are
programming libraries which are made for free to the public which provide
methods for performing certain actions via your own programs. Typically you
simply add the library to a program folder in your program, and then use some
sort of command to include that library either globally (available to the whole
program), or inside of a specific script where you intend to use the functions
of that library.

Many programming languages have a package manager program available which aids
in the retrieval and installation of these libraries. For the Perl programming
language there is a package manager and repository of libraries made available
called [Comprehensive Perl Archive Network (CPAN)].

For the PHP programming language there is a package manager and repository of
libraries made available through a system known as
[PHP Extension and Application Repository (PEAR)].

[RubyGems](http://en.wikipedia.org/wiki/RubyGems) is a package manager for the
Ruby programming language which provides program or libraries in packages
called 'Gems'. Ruby on Rails itself is a Gem, as are the many libraries which
the Rails framework relies on (ActiveModel, ActiveResource, ActiveSupport, etc).
After installing a gem, the Ruby interpreter has access to the gem by simply
including it. For instance a Ruby script that needs to use the functions
contained in the FasterCSV gem simply includes "require 'faster_csv'" at the
top of the script.

If you're using Mac OS X Leopard or above, you already have Ruby and Ruby Gems
running on your computer.

## Installing Rails

All you need to do is open up the Terminal program from your Applications folder
under 'Utilities'. We recommend that you drag the Terminal to your toolbar as
you'll be using it often enough as you work on your Ruby on Rails application
to make it easily accessible. From the command line of the Terminal
program run the command 'sudo gem update rails'. The lines displayed after this
command, after you've typed your password and pressed enter, should look similar
to this:

```bash
$ sudo gem update rails

Password:
Updating installed gems
Updating jquery-rails
Fetching: jquery-rails-1.0.16.gem (100%)
Fetching: activesupport-3.1.1.gem (100%)
Fetching: activemodel-3.1.1.gem (100%)
Fetching: rack-cache-1.1.gem (100%)
Fetching: actionpack-3.1.1.gem (100%)
Successfully installed jquery-rails-1.0.16
Successfully installed activesupport-3.1.1
Successfully installed activemodel-3.1.1
Successfully installed rack-cache-1.1
Successfully installed actionpack-3.1.1
Updating rails
Fetching: activerecord-3.1.1.gem (100%)
Fetching: activeresource-3.1.1.gem (100%)
Fetching: actionmailer-3.1.1.gem (100%)
Fetching: railties-3.1.1.gem (100%)
Fetching: rails-3.1.1.gem (100%)
Successfully installed activerecord-3.1.1
Successfully installed activeresource-3.1.1
Successfully installed actionmailer-3.1.1
Successfully installed railties-3.1.1
Successfully installed rails-3.1.1
Updating rails-footnotes
Fetching: rails-footnotes-3.7.5.gem (100%)
Successfully installed rails-footnotes-3.7.5
Gems updated: jquery-rails, activesupport, activemodel, rack-cache, actionpack, activerecord, activeresource, actionmailer, railties, rails, rails-footnotes
Installing ri documentation for jquery-rails-1.0.16...
Installing ri documentation for activesupport-3.1.1...
Installing ri documentation for activemodel-3.1.1...
Installing ri documentation for rack-cache-1.1...
Installing ri documentation for actionpack-3.1.1...
Installing ri documentation for activerecord-3.1.1...
Installing ri documentation for activeresource-3.1.1...
Installing ri documentation for actionmailer-3.1.1...
Installing ri documentation for railties-3.1.1...
Installing ri documentation for rails-3.1.1...
file 'lib' not found
Installing ri documentation for rails-footnotes-3.7.5...
Installing RDoc documentation for jquery-rails-1.0.16...
Installing RDoc documentation for activesupport-3.1.1...
Installing RDoc documentation for activemodel-3.1.1...
Installing RDoc documentation for rack-cache-1.1...
Installing RDoc documentation for actionpack-3.1.1...
Installing RDoc documentation for activerecord-3.1.1...
Installing RDoc documentation for activeresource-3.1.1...
Installing RDoc documentation for actionmailer-3.1.1...
Installing RDoc documentation for railties-3.1.1...
Installing RDoc documentation for rails-3.1.1...
file 'lib' not found
Installing RDoc documentation for rails-footnotes-3.7.5...
```

Note: You will not see stars or other characters as you enter your password.
Simply type it and press the 'Enter' key. If you mistyped the password, try
again until it works.

[PHP Extension and Application Repository (PEAR)]: http://pear.php.net/
[Comprehensive Perl Archive Network (CPAN)]: http://www.cpan.org/modules/INSTALL.html
[Textmate]: http://macromates.com/
[jEdit]: http://www.jedit.org/
[Aptana Radrails]: http://www.aptana.com/products/radrails/download
