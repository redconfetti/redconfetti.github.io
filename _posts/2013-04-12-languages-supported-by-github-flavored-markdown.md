---
layout: post
title: Languages Supported by Github Flavored Markdown
date: '2013-04-12 20:57:57 -0700'
categories:
- Gems
tags:
- yardoc
- github
- markdown
comments: []
---
I'm currently configuring the [Yard documentation](https://github.com/lsegal/yard) tool for use with Ruby/Rails projects. I could see that it's possible to create a [.yardopts file](https://github.com/lsegal/yard/blob/master/.yardopts) in the main directory for your Rails application, and simply add command line arguments to the file.

I just discovered that you can add a list of files, likely placed under the 'doc' directory, to your .yardopts file, and those files will be included in your generated documentation set. This is perfect for changelogs, readme files, and other high level documentation. After installing the Redcarpet gem, you can name these files with the '.md' extension to use [Markdown formatting](http://daringfireball.net/projects/markdown/syntax) on your documentation.

After further investigation I discovered that Yard supports [Github Flavored Markdown](https://help.github.com/articles/github-flavored-markdown), with support for syntax highlighting of a number of different languages. This is accomplished by wrapping your code with lines that consist of three backticks, with the first line suffixed by the language name.

``` ruby
this = "Ruby Code"
puts "This is #{this}"
```

Unfortunately the Github docs refer you to this [hightlight.js test page](http://softwaremaniacs.org/media/soft/highlight/test.html) for the list of supported languages.

Here is the list of languages that can be used with the backtick

* markdown
* Ruby
* PHP
* Perl
* python
* profile - python profiler output
* xml - XML and also used for HTML with inline CSS and Javascript
* css
* json
* javascript
* coffeescript
* django
* apache
* sql
* java
* delphi
* applescript
* cpp - C++
* objectivec
* ini
* cs
* vala
* d - RDMD
* rsl - RenderMan RSL
* rib - RenderMan RIB
* mel - Maya Embedded Language
* smalltalk
* lisp
* clojure
* nginx
* diff
* dos - dos batch files
* bash
* cmake
* axapta
* glsl
* lc
* avrasm - AVR Assembler
* vhdl
* parser3
* tex
* brainfuck
* haskell
* erlang
* erlang-repl
* rust
* matlab
* r
