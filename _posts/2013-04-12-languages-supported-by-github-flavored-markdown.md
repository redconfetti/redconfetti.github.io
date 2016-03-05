---
layout: post
status: publish
published: true
title: Languages Supported by Github Flavored Markdown
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1488
wordpress_url: http://www.rubycoloredglasses.com/?p=1488
date: '2013-04-12 20:57:57 -0700'
date_gmt: '2013-04-12 20:57:57 -0700'
categories:
- Gems
tags:
- yardoc
- github
- markdown
comments: []
---
<p>I'm currently configuring the <a href="https://github.com/lsegal/yard" target="_blank">Yard documentation</a> tool for use with Ruby/Rails projects. I could see that it's possible to create a <a href="https://github.com/lsegal/yard/blob/master/.yardopts" target="_blank">.yardopts file</a> in the main directory for your Rails application, and simply add command line arguments to the file.</p>
<p>I just discovered that you can add a list of files, likely placed under the 'doc' directory, to your .yardopts file, and those files will be included in your generated documentation set. This is perfect for changelogs, readme files, and other high level documentation. After installing the Redcarpet gem, you can name these files with the '.md' extension to use <a href="http://daringfireball.net/projects/markdown/syntax" target="_blank">Markdown formatting</a> on your documentation.</p>
<p>After further investigation I discovered that Yard supports <a href="https://help.github.com/articles/github-flavored-markdown" target="_blank">Github Flavored Markdown</a>, with support for syntax highlighting of a number of different languages. This is accomplished by wrapping your code with lines that consist of three backticks, with the first line suffixed by the language name.</p>
<pre class="brush:ruby">```ruby<br />
this = "Ruby Code"<br />
puts "This is #{this}"<br />
```</pre><br />
Unfortunately the Github docs refer you to this <a href="http://softwaremaniacs.org/media/soft/highlight/test.html" target="_blank">hightlight.js test page</a> for the list of supported languages.</p>
<p>Here is the list of languages that can be used with the backtick</p>
<ul>
<li>markdown</li>
<li><span style="line-height: 12px;">Ruby</span></li>
<li>PHP</li>
<li>Perl</li>
<li>python</li>
<li>profile - python profiler output</li>
<li>xml - XML and also used for HTML with inline CSS and Javascript</li>
<li>css</li>
<li>json</li>
<li>javascript</li>
<li>coffeescript</li>
<li>django</li>
<li>apache</li>
<li>sql</li>
<li>java</li>
<li>delphi</li>
<li>applescript</li>
<li>cpp - C++</li>
<li>objectivec</li>
<li>ini</li>
<li>cs</li>
<li>vala</li>
<li>d - RDMD</li>
<li>rsl - RenderMan RSL</li>
<li>rib - RenderMan RIB</li>
<li>mel - Maya Embedded Language</li>
<li>smalltalk</li>
<li>lisp</li>
<li>clojure</li>
<li>nginx</li>
<li>diff</li>
<li>dos - dos batch files</li>
<li>bash</li>
<li>cmake</li>
<li>axapta</li>
<li>glsl</li>
<li>lc</li>
<li>avrasm - AVR Assembler</li>
<li>vhdl</li>
<li>parser3</li>
<li>tex</li>
<li>brainfuck</li>
<li>haskell</li>
<li>erlang</li>
<li>erlang-repl</li>
<li>rust</li>
<li>matlab</li>
<li>r</li><br />
</ul></p>
