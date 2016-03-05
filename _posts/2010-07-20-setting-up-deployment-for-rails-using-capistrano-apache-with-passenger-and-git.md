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
<p>I don't have time right now to learn how to setup Capistrano. I just want a recipe that works and does the job. Here are my notes.</p>
<p>1) First install the Capistrano gem</p>
<pre class="brush:bash">sudo gem install capistrano</pre></p>
<p>2) Next you need to go into the directory of your Ruby on Rails application and capify it:</p>
<pre class="brush:bash">capify .</pre></p>
<p>3) Next I recommend this article (I'll rip off the deploy.rb soon and post it here)</p>
<p><a href="http://www.zorched.net/2008/06/17/capistrano-deploy-with-git-and-passenger/" target="_blank">http://www.zorched.net/2008/06/17/capistrano-deploy-with-git-and-passenger/</a></p>
<p>4) Once you've configured your deploy.rb, run this command to have it setup the directories on the remote server (releases, shared, logs, etc).</p>
<pre class="brush:bash">cap deploy:setup</pre></p>
<p>5) Next run this command to get the list of other capistrano commands you can run:</p>
<pre class="brush:bash">cap -T</pre></p>
<p>The output should look like</p>
<pre class="brush:bash">
cap deploy               # Deploys your project.<br />
cap deploy:check         # Test deployment dependencies.<br />
cap deploy:cleanup       # Clean up old releases.<br />
cap deploy:cold          # Deploys and starts a `cold' application.<br />
cap deploy:migrate       # Run the migrate rake task.<br />
cap deploy:migrations    # Deploy and run pending migrations.<br />
cap deploy:pending       # Displays the commits since your last deploy.<br />
cap deploy:pending:diff  # Displays the `diff' since your last deploy.<br />
cap deploy:restart       # Restarting mod_rails with restart.txt<br />
cap deploy:rollback      # Rolls back to a previous version and restarts.<br />
cap deploy:rollback:code # Rolls back to the previously deployed version.<br />
cap deploy:setup         # Prepares one or more servers for deployment.<br />
cap deploy:start         # start task is a no-op with mod_rails<br />
cap deploy:stop          # stop task is a no-op with mod_rails<br />
cap deploy:symlink       # Updates the symlink to the most recently deployed ...<br />
cap deploy:update        # Copies your project and updates the symlink.<br />
cap deploy:update_code   # Copies your project to the remote servers.<br />
cap deploy:upload        # Copy files to the currently deployed version.<br />
cap deploy:web:disable   # Present a maintenance page to visitors.<br />
cap deploy:web:enable    # Makes the application web-accessible again.<br />
cap invoke               # Invoke a single command on the remote servers.<br />
cap shell                # Begin an interactive Capistrano session.<br />
</pre></p>
