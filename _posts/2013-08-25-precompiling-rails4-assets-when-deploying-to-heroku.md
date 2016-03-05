---
layout: post
status: publish
published: true
title: Precompiling Rails 4 Assets When Deploying to Heroku
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1632
wordpress_url: http://www.rubycoloredglasses.com/?p=1632
date: '2013-08-25 00:13:01 -0700'
date_gmt: '2013-08-25 00:13:01 -0700'
categories:
- Ruby on Rails
tags:
- heroku
- rails4
- asset pipeline
---
<p>I'm working on a Rails 4.0.0 application, using Ruby 2.0.0 for a code challenge I'm working on. Part of this challenge is to deploy my application to Heroku. I haven't done this before, as I'm accustomed to deploying to a VPS with Capistrano.</p>
<p>Upon my first deploy I discovered that my assets weren't compiling. It wasn't even mentioned in the output while deploying. I reviewed the Heroku article on the <a href="https://devcenter.heroku.com/articles/rails-asset-pipeline" target="_blank">Rails asset pipeline</a>, but this didn't offer me any details to resolve my issue.</p>
<p>I discovered that I should <a href="https://devcenter.heroku.com/articles/rails4#logging-and-assets" target="_blank">add the rails_12factor gem</a> to my production gem group in the Gemfile. Here's a pretty annotated version that I used.</p>
<pre class="brush:ruby">##################<br />
# Production</p>
<p>group :production do</p>
<p>  # Rails 12factor<br />
  # Makes running your Rails app easier. Based on the ideas behind 12factor.net<br />
  # Needed for support of Asset Pipeline with Heroku<br />
  # https://github.com/heroku/rails_12factor<br />
  gem 'rails_12factor'</p>
<p>end</pre><br />
After doing this I ran the rake task to precompile assets, committed the changes to my repository, then deployed. This resolved the issue, at least in the short term. Having to precompile assets before each deploy isn't very efficient though, so I ran the task to clobber all the assets. If you don't clear all the precompiled assets using 'clobber', Heroku will not attempt to precompile the assets.</p>
<pre class="brush:shell">rake assets:precompile</p>
<p>rake assets:clobber</pre><br />
I tried many other asset pipeline settings in config/application.rb and config/environments/production.rb that were recommended in various <a href="http://stackoverflow.com/questions/15354539/heroku-does-not-compile-files-under-assets-piplines-in-rails-4" target="_blank">StackOverflow</a> threads, but nothing was working. I don't recall when, but at some point I finally started to see that Heroku was trying to precompile assets when I deployed.</p>
<pre class="brush:shell">       Your bundle is complete! It was installed into ./vendor/bundle<br />
       Cleaning up the bundler cache.<br />
-----> Writing config/database.yml to read from DATABASE_URL<br />
       Error detecting the assets:precompile task<br />
-----> Discovering process types</pre><br />
I ran the command to detect the rake tasks on the remote server, and 'assets:precompile' was showing up. I didn't understand why it's saying that it can't detect the task when it's right there on the remote server.</p>
<pre class="brush:shell">$ heroku run rake -T<br />
Running `rake -T` attached to terminal... up, run.1071<br />
rake about                  # List versions of all Rails frameworks and the environment<br />
rake assets:clean           # Remove old compiled assets<br />
rake assets:clobber         # Remove compiled assets<br />
rake assets:environment     # Load asset compile environment<br />
rake assets:precompile      # Compile all the assets named in config.assets.precompile</pre><br />
After further investigating I found <a href="https://github.com/smockle/smockle/issues/4" target="_blank">this post</a>. It turns out that if your applications configuration variables need to be present during the compilation of the "slug" being compiled, an add-on called <a href="https://devcenter.heroku.com/articles/labs-user-env-compile" target="_blank">user-env-compile</a> might help your application during deployment.</p>
<p>I'm using Figaro to load application configuration values, including the asset hostname.</p>
<pre class="brush:ruby"># config/application.rb<br />
config.action_controller.asset_host = 'http://' + Figaro.env.hostname</pre><br />
This must be the reason it was failing to attempt the precompiling of assets during deployment.</p>
<pre class="brush:shell">$ heroku labs:enable user-env-compile -a myapp</pre><br />
I installed user-env-compile and now it's working just fine.</p>
