---
layout: post
title: Languages Supported by Github Flavored Markdown
date: '2013-04-12 20:57:57 -0700'
comments: true
categories:
- Gems
tags:
- yardoc
- github
- markdown
---

__NOTE:__ This post updated on 12/08/2020

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

<!--more-->

After further investigation I discovered that Yard supports
[Github Flavored Markdown][4], with support for syntax highlighting of a
number of different languages. This is accomplished by wrapping your code with
lines that consist of three backticks, with the first line suffixed by the
language name.

````markdown
```ruby
this = "Ruby Code"
puts "This is #{this}"
```
````

Unfortunately the Github docs refer you to this [hightlight.js test page][5]
for the list of supported languages.

Github uses Linguist to perform language detection and syntax highlighting.
Here a list of common languages that can be used with the backtick (see
full list in [Linguist - languages.yml][6]).

* actionscript3
* apache
* applescript
* asp
* brainfuck
* c
* cfm
* clojure
* cmake
* coffee-script, coffeescript, coffee
* cpp - C++
* cs
* csharp
* css
* csv
* bash
* diff
* elixir
* erb - HTML + Embedded Ruby
* go
* haml
* http
* java
* javascript
* json
* jsx
* less
* lolcode
* make - Makefile
* markdown
* matlab
* nginx
* objectivec
* pascal
* PHP
* Perl
* python
* profile - python profiler output
* rust
* salt, saltstate - Salt
* shell, sh, zsh, bash - Shell scripting
* scss
* sql
* svg
* swift
* rb, jruby, ruby - Ruby
* smalltalk
* vim, viml - Vim Script
* volt
* vhdl
* vue
* xml - XML and also used for HTML with inline CSS and Javascript
* yaml

[1]: https://github.com/lsegal/yard
[2]: https://github.com/lsegal/yard/blob/master/.yardopts
[3]: http://daringfireball.net/projects/markdown/syntax
[4]: https://help.github.com/articles/github-flavored-markdown
[5]: https://highlightjs.readthedocs.io/en/latest/supported-languages.html
[6]: https://github.com/github/linguist/blob/master/lib/linguist/languages.yml
