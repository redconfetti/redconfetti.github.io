---
layout: post
title: Example Rake Task
date: '2012-01-01 18:26:28 -0800'
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
Here is example rake task code which you can use and modify the next time you're setting up new Rake tasks for a Rails app from scratch. The example includes the syntax for setting the default task, running multiple tasks in order, and a task which includes multiple arguments. This coding syntax works with Rails 3.1.

<pre class="brush:rails"># /lib/tasks/mytask.rake

namespace :mytask do

  task :default => 'mytask:full_run'

  desc "run all tasks in proper order"

  task :full_run => [:example_task_one, :example_task_two] do

    puts "All tasks ran and completed at #{Time.now}"

  end

  desc "example task without arguments"

  task :example_task_one => :environment do

    # task code goes here

    puts "Example Task One completed"

  end

  desc "example task with arguments"

  task :example_task_two, [:arg1, :arg2] => :environment do |t, args|

    # if no arguments, display docs

    if args.count == 0

      puts ''

      puts "rake mytask:example_task[arg1,arg2]"

      puts "arg1: description of first argument"

      puts "arg2: description of second argument"

      puts ''

      puts "example:"

      puts "rake mytask:example_task[123,'456']"

      puts ''

    else

      # task code goes here

      puts "Running task with arg1: #{args.arg1}, and arg2: #{args.arg2}"

      puts "Example Task Two completed"

    end

  end

end```

Special thanks to Jason Seifer for his <a href="http://jasonseifer.com/2010/04/06/rake-tutorial" target="_blank">Rake Tutorial</a> which made this possible.

