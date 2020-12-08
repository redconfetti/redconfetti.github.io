---
layout: post
title: Setting up Deployment for Rails using Capistrano, Apache with Passenger and
  Git
date: '2010-07-20 23:48:23 -0700'
categories:
- Ruby on Rails
tags:
- rails
- capistrano
- git
- passenger
comments: []
---

I don't have time right now to learn how to setup Capistrano. I just want a
recipe that works and does the job. Here are my notes.

1. First install the Capistrano gem

```bash
sudo gem install capistrano
```

1. Next you need to go into the directory of your Ruby on Rails application
and capify it:

```bash
capify .
```

<!--more-->

1. Next I recommend this article (I'll rip off the deploy.rb soon and post it
here)

[Capistrano Deploy with Git and Passenger]

1. Once you've configured your deploy.rb, run this command to have it setup
the directories on the remote server (releases, shared, logs, etc).

```bash
cap deploy:setup
```

1. Next run this command to get the list of other capistrano commands you can
run:

``` shell
cap -T
```

The output should look like

``` shell
cap deploy               # Deploys your project.
cap deploy:check         # Test deployment dependencies.
cap deploy:cleanup       # Clean up old releases.
cap deploy:cold          # Deploys and starts a `cold' application.
cap deploy:migrate       # Run the migrate rake task.
cap deploy:migrations    # Deploy and run pending migrations.
cap deploy:pending       # Displays the commits since your last deploy.
cap deploy:pending:diff  # Displays the `diff' since your last deploy.
cap deploy:restart       # Restarting mod_rails with restart.txt
cap deploy:rollback      # Rolls back to a previous version and restarts.
cap deploy:rollback:code # Rolls back to the previously deployed version.
cap deploy:setup         # Prepares one or more servers for deployment.
cap deploy:start         # start task is a no-op with mod_rails
cap deploy:stop          # stop task is a no-op with mod_rails
cap deploy:symlink       # Updates the symlink to the most recently deployed ...
cap deploy:update        # Copies your project and updates the symlink.
cap deploy:update_code   # Copies your project to the remote servers.
cap deploy:upload        # Copy files to the currently deployed version.
cap deploy:web:disable   # Present a maintenance page to visitors.
cap deploy:web:enable    # Makes the application web-accessible again.
cap invoke               # Invoke a single command on the remote servers.
cap shell                # Begin an interactive Capistrano session.
```

[Capistrano Deploy with Git and Passenger]: http://www.zorched.net/2008/06/17/capistrano-deploy-with-git-and-passenger/
