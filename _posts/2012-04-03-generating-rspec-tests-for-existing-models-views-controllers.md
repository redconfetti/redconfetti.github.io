---
layout: post
title: Generating Test File Stubs for Existing Models, Views, and Controllers
date: '2012-04-03 13:10:12 -0700'
categories:
- Testing
tags:
- rails3.1
- testing
- rspec
- tdd
comments: []
---
<p>I've noticed that if you install certain testing gems, like Factory Girl, or Rspec, that your Rails application will create test files for these libraries instead of using the defaults. Even further you can configure the generators used by your Rails app in /config/application.rb</p>
<pre class="brush:rails"># Configure generators values. Many other options are available, be sure to check the documentation.<br />
# http://edgeguides.rubyonrails.org/generators.html#customizing-your-workflow<br />
config.generators do |g|<br />
  g.stylesheets false<br />
  g.test_framework :rspec<br />
  g.fallbacks[:rspec] = :test_unit<br />
  g.fixture_replacement :factory_girl<br />
end</pre><br />
I've been anxious however in deciding which testing tools to learn and use with my project. If I choose the wrong one, then all the scaffold generated test code will be generated for the test framework I might choose to quit using at some point.</p>
<p>This is not completely correct though, as I've discovered.</p>
<p>I found that the following commands will generate the empty spec files.</p>
<pre class="brush:shell">rails generate rspec:model<br />
rails generate rspec:view<br />
rails generate rspec:controller</pre><br />
Even better though, if you're looking for scaffold style files to be put in place, use the following syntax to generate scaffold code.</p>
<pre class="brush:shell">rails generate rspec:scaffold Post title:string body:text</pre><br />
If you decide to use basic Test::Unit based tests, instead of going with Rspec, you can also reconfigure your app to use Test::Unit again with generators, and then use the rake commands to generate the files that were generated via rspec.</p>
<pre class="brush:shell">$ rails g<br />
Usage: rails generate GENERATOR [args] [options]</p>
<p>General options:<br />
  -h, [--help]     # Print generator's options and usage<br />
  -p, [--pretend]  # Run but do not make any changes<br />
  -f, [--force]    # Overwrite files that already exist<br />
  -s, [--skip]     # Skip files that already exist<br />
  -q, [--quiet]    # Suppress status output</p>
<p>Please choose a generator below.</p>
<p>TestUnit:<br />
  test_unit:controller<br />
  test_unit:helper<br />
  test_unit:integration<br />
  test_unit:mailer<br />
  test_unit:model<br />
  test_unit:observer<br />
  test_unit:performance<br />
  test_unit:plugin<br />
  test_unit:scaffold</pre></p>
