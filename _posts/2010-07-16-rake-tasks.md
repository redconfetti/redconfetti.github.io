---
layout: post
title: Rake Tasks
date: '2010-07-16 17:37:56 -0700'
categories:
- Ruby on Rails
tags: []
comments: []
---
<p>If you're wanting to know which Rake tasks are available for you to use from the command line, simply use the 'rake -T' command:</p>
<pre class="brush:bash">
macbook:railsproject jason$ rake -T<br />
(in /Users/jason/railsproject)<br />
rake db:abort_if_pending_migrations       # Raises an error if there are pending migrations<br />
rake db:charset                           # Retrieves the charset for the current environment's database<br />
rake db:collation                         # Retrieves the collation for the current environment's database<br />
rake db:create                            # Create the database defined in config/database.yml for the current RAILS_ENV<br />
rake db:create:all                        # Create all the local databases defined in config/database.yml<br />
rake db:drop                              # Drops the database for the current RAILS_ENV<br />
rake db:drop:all                          # Drops all the local databases defined in config/database.yml<br />
</pre></p>
<p>A really useful one is the 'routes' option which outputs a list of the routes configured.</p>
<pre class="brush:bash">
macbook:railsproject jason$ rake routes<br />
(in /Users/jason/railsproject)<br />
  /:controller/:action/:id<br />
  /:controller/:action/:id(.:format)<br />
</pre></p>
