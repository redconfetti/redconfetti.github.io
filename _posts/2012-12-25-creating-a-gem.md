---
layout: post
title: Creating a Gem
date: '2012-12-25 09:50:03 -0800'
categories:
- Gems
tags:
- gem
comments: []
---
<p>In the past gems were created manually, or generated using the <a href="http://rubygems.org/gems/echoe" target="_blank">echoe gem</a> (last release Sept 21, 2011), or the <a href="https://github.com/appoxy/jeweler" target="_blank">Jeweler gem</a> (last release November 7, 2011).</p>
<p>Since then it appears that the most automated way to create a gem is by using Bundler, via the '<a href="http://gembundler.com/v1.2/bundle_gem.html" target="_blank">bundle gem</a>' command.</p>
<pre class="brush:rails">
$ bundle gem my_tools<br />
      create  my_tools/Gemfile<br />
      create  my_tools/Rakefile<br />
      create  my_tools/LICENSE<br />
      create  my_tools/README.md<br />
      create  my_tools/.gitignore<br />
      create  my_tools/my_tools.gemspec<br />
      create  my_tools/lib/my_tools.rb<br />
      create  my_tools/lib/my_tools/version.rb<br />
Initializating git repo in /Users/jsmith/Sites/my_tools<br />
</pre></p>
