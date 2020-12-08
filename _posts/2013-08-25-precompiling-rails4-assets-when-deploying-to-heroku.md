---
layout: post
title: Precompiling Rails 4 Assets When Deploying to Heroku
date: '2013-08-25 00:13:01 -0700'
categories:
- Ruby on Rails
tags:
- heroku
- rails4
- asset pipeline
---

I'm working on a Rails 4.0.0 application, using Ruby 2.0.0 for a code challenge
I'm working on. Part of this challenge is to deploy my application to Heroku. I
haven't done this before, as I'm accustomed to deploying to a VPS with
Capistrano.

Upon my first deploy I discovered that my assets weren't compiling. It wasn't
even mentioned in the output while deploying. I reviewed the Heroku article on
the [Rails asset pipeline], but this didn't offer me any details to resolve my
issue.

I discovered that I should [add the rails_12factor gem] to my production gem
group in the Gemfile. Here's a pretty annotated version that I used.

<!--more-->

``` ruby
# Production
group :production do
  # Rails 12factor
  # Makes running your Rails app easier. Based on the ideas behind 12factor.net
  # Needed for support of Asset Pipeline with Heroku
  # https://github.com/heroku/rails_12factor
  gem 'rails_12factor'
end
```

After doing this I ran the rake task to precompile assets, committed the changes
to my repository, then deployed. This resolved the issue, at least in the short
term. Having to precompile assets before each deploy isn't very efficient though,
so I ran the task to clobber all the assets. If you don't clear all the
precompiled assets using 'clobber', Heroku will not attempt to precompile the
assets.

``` shell
rake assets:precompile
rake assets:clobber
```

I tried many other asset pipeline settings in config/application.rb and
config/environments/production.rb that were recommended in various
[StackOverflow] threads, but nothing was working. I don't recall when, but at
some point I finally started to see that Heroku was trying to precompile assets
when I deployed.


``` shell
       Your bundle is complete! It was installed into ./vendor/bundle
       Cleaning up the bundler cache.
-----> Writing config/database.yml to read from DATABASE_URL
       Error detecting the assets:precompile task
-----> Discovering process types
```

I ran the command to detect the rake tasks on the remote server, and
'assets:precompile' was showing up. I didn't understand why it's saying that it
can't detect the task when it's right there on the remote server.

``` shell
$ heroku run rake -T
Running `rake -T` attached to terminal... up, run.1071
rake about                  # List versions of all Rails frameworks and the environment
rake assets:clean           # Remove old compiled assets
rake assets:clobber         # Remove compiled assets
rake assets:environment     # Load asset compile environment
rake assets:precompile      # Compile all the assets named in config.assets.precompile
```

After further investigating I found [this post]. It turns out that if your
applications configuration variables need to be present during the compilation
of the "slug" being compiled, an add-on called [user-env-compile] might help
your application during deployment.

I'm using Figaro to load application configuration values, including the asset
hostname.


```ruby
# config/application.rb
config.action_controller.asset_host = 'http://' + Figaro.env.hostname
```

This must be the reason it was failing to attempt the precompiling of assets
during deployment.

``` shell
$ heroku labs:enable user-env-compile -a myapp
```

I installed user-env-compile and now it's working just fine.

[user-env-compile]: https://devcenter.heroku.com/articles/labs-user-env-compile
[this post]: https://github.com/smockle/smockle/issues/4
[StackOverflow]: http://stackoverflow.com/questions/15354539/heroku-does-not-compile-files-under-assets-piplines-in-rails-4
[add the rails_12factor gem]: https://devcenter.heroku.com/articles/rails4#logging-and-assets
[Rails asset pipeline]: https://devcenter.heroku.com/articles/rails-asset-pipeline
