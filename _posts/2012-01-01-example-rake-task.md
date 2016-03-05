---
layout: post
status: publish
published: true
title: Example Rake Task
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 943
wordpress_url: http://www.redconfetti.com/?p=943
date: '2012-01-01 18:26:28 -0800'
date_gmt: '2012-01-01 22:26:28 -0800'
categories:
- Ruby on Rails
tags:
- rake
comments:
- id: 334
  author: Jason Seifer
  author_email: me@jasonseifer.com
  author_url: http://jasonseifer.com
  date: '2012-01-02 17:32:34 -0800'
  date_gmt: '2012-01-02 21:32:34 -0800'
  content: Thanks for the shout out!
---
<p>Here is example rake task code which you can use and modify the next time you're setting up new Rake tasks for a Rails app from scratch. The example includes the syntax for setting the default task, running multiple tasks in order, and a task which includes multiple arguments. This coding syntax works with Rails 3.1.</p>
<pre class="brush:rails"># /lib/tasks/mytask.rake<br />
namespace :mytask do</p>
<p>  task :default => 'mytask:full_run'</p>
<p>  desc "run all tasks in proper order"<br />
  task :full_run => [:example_task_one, :example_task_two] do<br />
    puts "All tasks ran and completed at #{Time.now}"<br />
  end</p>
<p>  desc "example task without arguments"<br />
  task :example_task_one => :environment do<br />
    # task code goes here<br />
    puts "Example Task One completed"<br />
  end</p>
<p>  desc "example task with arguments"<br />
  task :example_task_two, [:arg1, :arg2] => :environment do |t, args|<br />
    # if no arguments, display docs<br />
    if args.count == 0<br />
      puts ''<br />
      puts "rake mytask:example_task[arg1,arg2]"<br />
      puts "arg1: description of first argument"<br />
      puts "arg2: description of second argument"<br />
      puts ''<br />
      puts "example:"<br />
      puts "rake mytask:example_task[123,'456']"<br />
      puts ''<br />
    else<br />
      # task code goes here<br />
      puts "Running task with arg1: #{args.arg1}, and arg2: #{args.arg2}"<br />
      puts "Example Task Two completed"<br />
    end<br />
  end<br />
end</pre><br />
Special thanks to Jason Seifer for his <a href="http://jasonseifer.com/2010/04/06/rake-tutorial" target="_blank">Rake Tutorial</a> which made this possible.</p>
