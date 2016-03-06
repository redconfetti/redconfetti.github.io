---
layout: post
title: Yard Documentation
date: '2013-08-31 21:50:47 -0700'
categories:
- Documentation
tags:
- Yard
---
Here are my own notes for using <a href="https://github.com/lsegal/yard" target="_blank">Yard</a> to provide the Ruby API documentation and other notes for your application.

## Installation

First add the Yard gem to your Gemfile, preferably in the development group if applicable.


``` ruby
group :development do
  # Yard
  # YARD is a Ruby Documentation tool
  # https://github.com/lsegal/yard
  gem "yard", "~> 0.8.7"
end
```

## Running

You can use Yard to generate documentation by just running 'Yard' from the root of your application.

``` shell
$ yard

Files:          36

Modules:        10 (   10 undocumented)

Classes:        26 (   21 undocumented)

Constants:       0 (    0 undocumented)

Methods:       140 (   54 undocumented)

 51.70% documented```

You can also run a server that updates dynamically as you add documentation.

``` shell
$ yard server

>> YARD 0.8.7 documentation server at http://0.0.0.0:8808
[2013-08-31 14:28:15] INFO  WEBrick 1.3.1
[2013-08-31 14:28:15] INFO  ruby 2.0.0 (2013-06-27) [x86_64-darwin12.4.1]
[2013-08-31 14:28:15] INFO  WEBrick::HTTPServer#start: pid=41901 port=8808
```

## Configuration

You can run the 'yardoc' command with options that cause it to parse certain directories for documentation. With Rails applications it appears that this isn't necessary. Rather than add options or flags after the yard command each time, you can configure a [.yardopts file](https://github.com/lsegal/yard/blob/master/.yardopts) with the arguments you would normally use from the command line.

Yard will make use of your README.md file as the index page for the documentation, but to include other files you could configure a .yardopts file like so:

``` shell
-
README.md
CHANGELOG.md
```

This makes it possible for the CHANGELOG to show up under the 'File List' section.

I prefer to have my own hierarchy of markdown files in /doc, with generated documentation in /doc/app. This way I can completed delete the doc/app folder without affecting my other markup files in the root of /doc.

``` shell
--output-dir doc/app
-
doc/DevelopmentTasks.md
CHANGELOG.md
README.md
```

Here is a good example of a more elaborately [configured .yardopts file](https://github.com/lsegal/yard/blob/master/.yardopts). You can also run 'yardoc --help' to discover other options to add to the file.
