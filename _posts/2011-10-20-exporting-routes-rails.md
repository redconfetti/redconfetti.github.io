---
layout: post
title: Exporting Routes in Rails 3
date: '2011-10-20 14:36:54 -0700'
comments: true
categories:
- Model
- Rake
---

I'm currently upgrading a project I'm working on from Rails 2.3.8 to Rails 3.1.
As part of this upgrade I need to test the entire application for issues,
because we haven't actually written any tests.

To help with this, I'd like to export all the routes so that I can test them
one by one, and keep track of what I've tested and fixed already.

Typically I output the routes by using this command from the root of the Rails
app:

``` shell
rake routes
```

<!--more-->

In this case I want to export all the routes into a format which I can post to
our wiki as a table. This will require a custom script, but I'm not sure how
Rails 3 internally stores routes.

As a start I found that rake will reveal the source of the 'routes' task using
this command

``` shell
$ rake --where routes

rake rails:upgrade:routes           /Users/jason/myapp/vendor/plugins/rails_upgrade/lib/tasks/rails_upgrade_tasks.rake:27
rake routes                         /opt/local/lib/ruby/gems/1.8/gems/railties-3.1.0/lib/rails/tasks/routes.rake:2
```

So it appears that what I'm looking for is in
/opt/local/lib/ruby/gems/1.8/gems/railties-3.1.0/lib/rails/tasks/routes.rake.
I've modified this task and added it to my application under
/lib/tasks/routes.rake like so:

```ruby
namespace :routes do

  desc 'Print out all defined routes in CSV format. Target specific controller with CONTROLLER=x.'
  task :csv => :environment do
    Rails.application.reload_routes!
    all_routes = Rails.application.routes.routes

    if ENV['CONTROLLER']
      all_routes = all_routes.select do |route|
        route.defaults[:controller] == ENV['CONTROLLER']
      end
    end

    routes = all_routes.collect do |route|
      reqs = route.requirements.dup
      reqs[:to] = route.app unless route.app.class.name.to_s =~ /^ActionDispatch::Routing/
      reqs = reqs.empty? ? "" : reqs.inspect

      {:name => route.name.to_s, :verb => route.verb.to_s, :path => route.path, :controller => route.requirements[:controller], :action => route.requirements[:action]}
    end

     # Skip the route if it's internal info route
    routes.reject! { |r| r[:path] =~ %r{/rails/info/properties|^/assets} }

    # name_width = routes.map{ |r| r[:name].length }.max
    # verb_width = routes.map{ |r| r[:verb].length }.max
    # path_width = routes.map{ |r| r[:path].length }.max

    puts "controller,action,method,path,name"

    routes.each do |r|
      puts "#{r[:controller]},#{r[:action]},#{r[:verb]},#{r[:path]},#{r[:name]}"
    end
  end
end
```

I can create a CSV file on my desktop, which contains all my routes by simply
running this command now:

```shell
rake routes:csv > ~/Desktop/rake-routes.csv
```
