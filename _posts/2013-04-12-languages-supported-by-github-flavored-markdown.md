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
---

I'm currently configuring the [Yard documentation][1] tool for use with
Ruby/Rails projects. I could see that it's possible to create a
[.yardopts file][2] in the main directory for your Rails application, and
simply add command line arguments to the file.

I just discovered that you can add a list of files, likely placed under the
'doc' directory, to your .yardopts file, and those files will be included in
your generated documentation set. This is perfect for changelogs, readme
files, and other high level documentation. After installing the Redcarpet gem,
you can name these files with the '.md' extension to use
[Markdown formatting][3] on your documentation.

After further investigation I discovered that Yard supports
[Github Flavored Markdown][4], with support for syntax highlighting of a
number of different languages. This is accomplished by wrapping your code with
lines that consist of three backticks, with the first line suffixed by the
language name.

``` ruby
this = "Ruby Code"
puts "This is #{this}"
```

Unfortunately the Github docs refer you to this [hightlight.js test page][5]
for the list of supported languages.

Here is the list of languages that can be used with the backtick

* apache
* applescript
* avrasm - AVR Assembler
* axapta
* brainfuck
* clojure
* cmake
* coffeescript
* cpp - C++
* cs
* css
* bash
* d - RDMD
* delphi
* diff
* dos - dos batch files
* django
* erlang
* erlang-repl
* glsl
* haskell
* ini
* java
* json
* javascript
* lc
* lisp
* markdown
* matlab
* mel - Maya Embedded Language
* nginx
* objectivec
* parser3
* PHP
* Perl
* python
* profile - python profiler output
* r
* rib - RenderMan RIB
* rsl - RenderMan RSL
* rust
* sql
* tex
* vala
* Ruby
* smalltalk
* vhdl
* xml - XML and also used for HTML with inline CSS and Javascript

[1]: https://github.com/lsegal/yard
[2]: https://github.com/lsegal/yard/blob/master/.yardopts
[3]: http://daringfireball.net/projects/markdown/syntax
[4]: https://help.github.com/articles/github-flavored-markdown
[5]: http://softwaremaniacs.org/media/soft/highlight/test.html
