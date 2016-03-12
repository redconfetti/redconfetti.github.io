---
layout: post
title: Rake Tasks
date: '2010-07-16 17:37:56 -0700'
categories:
- Ruby on Rails
tags: []
comments: []
---
If you're wanting to know which Rake tasks are available for you to use from the command line, simply use the 'rake -T' command:

``` shell
$ rake -T
(in /Users/jason/railsproject)
rake db:abort_if_pending_migrations       # Raises an error if there are pending migrations
rake db:charset                           # Retrieves the charset for the current environment's database
rake db:collation                         # Retrieves the collation for the current environment's database
rake db:create                            # Create the database defined in config/database.yml for the current RAILS_ENV
rake db:create:all                        # Create all the local databases defined in config/database.yml
rake db:drop                              # Drops the database for the current RAILS_ENV
rake db:drop:all                          # Drops all the local databases defined in config/database.yml
```

A really useful one is the 'routes' option which outputs a list of the routes configured.

``` shell
macbook:railsproject jason$ rake routes
(in /Users/jason/railsproject)
  /:controller/:action/:id
  /:controller/:action/:id(.:format)
```

