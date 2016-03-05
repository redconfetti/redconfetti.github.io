---
layout: post
status: publish
published: true
title: Yard Documentation
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1650
wordpress_url: http://www.rubycoloredglasses.com/?p=1650
date: '2013-08-31 21:50:47 -0700'
date_gmt: '2013-08-31 21:50:47 -0700'
categories:
- Documentation
tags:
- Yard
---
<p>Here are my own notes for using <a href="https://github.com/lsegal/yard" target="_blank">Yard</a> to provide the Ruby API documentation and other notes for your application.</p>
<h2>Installation</h2><br />
First add the Yard gem to your Gemfile, preferably in the development group if applicable.</p>
<pre class="brush:ruby">group :development do<br />
  # Yard<br />
  # YARD is a Ruby Documentation tool<br />
  # https://github.com/lsegal/yard<br />
  gem "yard", "~> 0.8.7"<br />
end</pre></p>
<h2>Running</h2><br />
You can use Yard to generate documentation by just running 'Yard' from the root of your application.</p>
<pre class="brush:shell">$ yard<br />
Files:          36<br />
Modules:        10 (   10 undocumented)<br />
Classes:        26 (   21 undocumented)<br />
Constants:       0 (    0 undocumented)<br />
Methods:       140 (   54 undocumented)<br />
 51.70% documented</pre><br />
You can also run a server that updates dynamically as you add documentation.</p>
<pre class="brush:shell">$ yard server<br />
>> YARD 0.8.7 documentation server at http://0.0.0.0:8808<br />
[2013-08-31 14:28:15] INFO  WEBrick 1.3.1<br />
[2013-08-31 14:28:15] INFO  ruby 2.0.0 (2013-06-27) [x86_64-darwin12.4.1]<br />
[2013-08-31 14:28:15] INFO  WEBrick::HTTPServer#start: pid=41901 port=8808</pre></p>
<h2>Configuration</h2><br />
You can run the 'yardoc' command with options that cause it to parse certain directories for documentation. With Rails applications it appears that this isn't necessary. Rather than add options or flags after the yard command each time, you can configure a <a href="https://github.com/lsegal/yard/blob/master/.yardopts" target="_blank">.yardopts file</a> with the arguments you would normally use from the command line.</p>
<p>Yard will make use of your README.md file as the index page for the documentation, but to include other files you could configure a .yardopts file like so:</p>
<pre class="brush:shell">-<br />
README.md<br />
CHANGELOG.md</pre><br />
This makes it possible for the CHANGELOG to show up under the 'File List' section.</p>
<p>I prefer to have my own hierarchy of markdown files in /doc, with generated documentation in /doc/app. This way I can completed delete the doc/app folder without affecting my other markup files in the root of /doc.</p>
<pre class="brush:shell">--output-dir doc/app<br />
-<br />
doc/DevelopmentTasks.md<br />
CHANGELOG.md<br />
README.md</pre><br />
Here is a good example of a more elaborately <a href="https://github.com/lsegal/yard/blob/master/.yardopts" target="_blank">configured .yardopts file</a>. You can also run 'yardoc --help' to discover other options to add to the file.</p>
