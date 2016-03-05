---
layout: post
status: publish
published: true
title: Exporting Routes in Rails 3
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 185
wordpress_url: http://rails.redconfetti.com/?p=185
date: '2011-10-20 14:36:54 -0700'
date_gmt: '2011-10-20 21:36:54 -0700'
categories:
- Model
- Rake
tags: []
comments: []
---
<p>I'm currently upgrading a project I'm working on from Rails 2.3.8 to Rails 3.1. As part of this upgrade I need to test the entire application for issues, because we haven't actually written any tests.</p>
<p>To help with this, I'd like to export all the routes so that I can test them one by one, and keep track of what I've tested and fixed already.</p>
<p>Typically I output the routes by using this command from the root of the Rails app:</p>
<pre class="brush:shell">
rake routes<br />
</pre></p>
<p>In this case I want to export all the routes into a format which I can post to our wiki as a table. This will require a custom script, but I'm not sure how Rails 3 internally stores routes.</p>
<p>As a start I found that rake will reveal the source of the 'routes' task using this command</p>
<pre class="brush:shell">
$ rake --where routes</p>
<p>rake rails:upgrade:routes           /Users/jason/myapp/vendor/plugins/rails_upgrade/lib/tasks/rails_upgrade_tasks.rake:27<br />
rake routes                         /opt/local/lib/ruby/gems/1.8/gems/railties-3.1.0/lib/rails/tasks/routes.rake:2<br />
</pre></p>
<p>So it appears that what I'm looking for is in /opt/local/lib/ruby/gems/1.8/gems/railties-3.1.0/lib/rails/tasks/routes.rake. I've modified this task and added it to my application under /lib/tasks/routes.rake like so:</p>
<pre class="brush:rails">
namespace :routes do</p>
<p>  desc 'Print out all defined routes in CSV format. Target specific controller with CONTROLLER=x.'<br />
  task :csv => :environment do<br />
    Rails.application.reload_routes!<br />
    all_routes = Rails.application.routes.routes</p>
<p>    if ENV['CONTROLLER']<br />
      all_routes = all_routes.select{ |route| route.defaults[:controller] == ENV['CONTROLLER'] }<br />
    end</p>
<p>    routes = all_routes.collect do |route|</p>
<p>      reqs = route.requirements.dup<br />
      reqs[:to] = route.app unless route.app.class.name.to_s =~ /^ActionDispatch::Routing/<br />
      reqs = reqs.empty? ? "" : reqs.inspect</p>
<p>      {:name => route.name.to_s, :verb => route.verb.to_s, :path => route.path, :controller => route.requirements[:controller], :action => route.requirements[:action]}<br />
    end</p>
<p>     # Skip the route if it's internal info route<br />
    routes.reject! { |r| r[:path] =~ %r{/rails/info/properties|^/assets} }</p>
<p>    # name_width = routes.map{ |r| r[:name].length }.max<br />
    # verb_width = routes.map{ |r| r[:verb].length }.max<br />
    # path_width = routes.map{ |r| r[:path].length }.max</p>
<p>    puts "controller,action,method,path,name"</p>
<p>    routes.each do |r|<br />
      puts "#{r[:controller]},#{r[:action]},#{r[:verb]},#{r[:path]},#{r[:name]}"<br />
    end<br />
  end<br />
end<br />
</pre></p>
<p>I can create a CSV file on my desktop, which contains all my routes by simply running this command now:</p>
<pre class="brush:shell">
$ rake routes:csv > ~/Desktop/rake-routes.csv<br />
</pre></p>
