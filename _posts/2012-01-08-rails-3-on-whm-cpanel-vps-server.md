---
layout: post
title: Rails 3 on WHM / cPanel VPS Server
date: '2012-01-08 13:14:58 -0800'
categories:
- Ruby on Rails
tags:
- capistrano
- passenger
- rails3.1
- cpanel
comments:
- id: 333
  author: Brook
  author_email: azzzz@gmx.net
  author_url: ''
  date: '2012-01-16 22:20:00 -0800'
  date_gmt: '2012-01-17 02:20:00 -0800'
  content: Thanks - been looking for something like this for ages!
- id: 28093
  author: Rudi
  author_email: rudi@webhostingzone.co.za
  author_url: http://www.webhostingzone.co.za
  date: '2014-03-20 11:37:30 -0700'
  date_gmt: '2014-03-20 11:37:30 -0700'
  content: Thanx for the tutorial. Which version of Ruby did you install this way?
    I want to run Ruby 2 on a cPanel server (currently 11.42.0). Will this work?
- id: 28156
  author: redconfetti
  author_email: jason@redconfetti.com
  author_url: http://www.redconfetti.com/
  date: '2014-03-20 21:28:31 -0700'
  date_gmt: '2014-03-20 21:28:31 -0700'
  content: Good question Rudi. When I did this, and documented the process in the
    article above, I used '/scripts/installruby', which is a script provided
    by cPanel. I'm not sure if it would install a different version today. When I
    wrote this article it installed Ruby 1.8.7.
---
cPanel is <a href="http://forums.cpanel.net/f145/mod_rails-passenger-instead-mongrel-rails-3-support-case-44197-a-152577-p3.html" target="_blank">working towards making Rails 3 applications run natively</a> with Passenger, setup via the cPanel interface. I'm not really sure if this will be ideal, as most organizations deploy their apps to the server using Capistrano, not uploading via FTP or something.

I've been hosting a number of PHP driven sites, including this blog, from a shared hosting service for quite a while now. Shared hosting is fine for personal websites or even small businesses with 4-5 page brochure style websites that do not receive lots of traffic, but they're not fine if slow performance or intermittent downtime causes you to loose business (or even the respect of your visitors). In such cases I recommend a VPS, because you control who you're hosting and thus can ensure optimal uptime and performance. I highly recommend <a href="http://www.linode.com/" target="_blank">Linode</a> as a VPS provider.

I've been using the shared hosting for PHP/Wordpress sites, and a VPS to host the Ruby on Rails applications I've been working on. Really this is expensive, so I'm wanting to consolidate to one VPS for everything.

I've been forewarned that cPanel does things it's own way, so if you're trying to do something out-of-the-box you can run into issues. I'm aware of this, and through this article will let you know if setting up a Rails 3.1.3 hosting environment is possible with a WHM / cPanel server (RELEASE version 11.30.5, build 3). I plan on using Gitosis under an account to host repositories, Capistrano for deployment, and Passenger with Apache2 already provided by cPanel.

## Installing Ruby

To stay within the "box" of the cPanel environment, I installed Ruby using the script provided by cPanel 

``` shell

/scripts/installruby

```

## Gitosis

To setup Gitosis you have to first install Python tools.

``` shell

yum -y install python-setuptools

```

Next, to install Git, you'll have to use a special command because cPanel has configured /etc/yum.conf to exclude certain packages, including perl packages, so that they do not break or conflict with the cPanel system. Use the following command to install Git:

``` shell

yum --disableexcludes=main install git

```

From the root home directory, download and install Gitosis.

``` shell

cd /root

git clone git://eagain.net/gitosis.git

cd gitosis

python setup.py install

```

Next create an account to host your Git repositories from the WHM interface. I've added a user with the user name 'git', and used 'git.web-app-host.com' as the domain (a subdomain under my hosting service domain). Set the password to a very long secure password. You won't be needing it again, as you'll be using an SSH key to authenticate.

After you're done creating the cPanel account which will host the repositories, copy your public key from your local machine to your root users home direcotry ( /root/ ).

``` shell

scp ~/.ssh/id_rsa.pub root@vps.web-app-host.com:/root

```

Go back to your SSH session as 'root' on the server and run this command to initialize the Gitosis repository under the 'git' user account.

``` shell

root@vps [~]# sudo -H -u git gitosis-init < /root/id_rsa.pub

Initialized empty Git repository in /home/git/repositories/gitosis-admin.git/

Reinitialized existing Git repository in /home/git/repositories/gitosis-admin.git/

```

NOTE: Do not use the 'Manage SSH Keys' option from the cPanel for the Git acccount, as this will remove the Gitosis-admin repository key from /home/git/.ssh/authorized_keys.

Run the following command to make sure the post-update hook is executable. If this isn't done, then tasks performed by Gitosis after you commit an update aren't performed (i.e. creating new repositories).

``` shell

sudo chmod u+x /home/git/repositories/gitosis-admin.git/hooks/post-update

```

On your local machine, run the following command to clone the Gitosis-admin repository, used to manage your repositories on the server, to your local machine.

``` shell

git clone git@<YOURSERVER>:gitosis-admin.git

```

This should look like this:

``` shell

$ git clone git@vps.web-app-host.com:gitosis-admin.git

Cloning into 'gitosis-admin'...

stdin: is not a tty

remote: Counting objects: 5, done.

remote: Compressing objects: 100% (5/5), done.

remote: Total 5 (delta 0), reused 5 (delta 0)

Receiving objects: 100% (5/5), done.

```

<hr />
Note: If you are prompted for a password when running this clone command, you likely have some sort of SSH configuration not setup properly on your local machine. If you're using multiple keys with various hosts, check ~/.ssh/config and make sure you're using the proper syntax. Run 'ssh git@<YOURSERVER> -v' to get a verbose output of what's happening when the SSH session is initialized to investigate further.

<hr />
On the remote machine, go ahead and delete your public key from the root users home directory.

``` shell

rm /root/id_rsa.pub

```

After cloning the Gitosis repository to your local machine, you simply modify and commit changes to the 'gitosis.conf' file inside of the 'gitosis-admin' folder.

If you're configuring new users, simply add their public SSH keys to the 'keydir' folder with the '.pub' file extension. Refer to these users using the filename of the public key file without the '.pub' extension.

For instance I've added a repository called 'marketsim', and then added 'marketsim' to the 'writable' setting for the gitosis-admin group.

<pre class="brush:text">
[gitosis]

[group gitosis-admin]

writable = gitosis-admin marketsim

members = jason@mymacbook.local

[repo marketsim]

gitweb = no

description = Market Simulation App

owner = Jason Miller

daemon = no

```

Alternatively I could create a new group with writable access to the 'marketsim' repository.

<pre class="brush:text">
[gitosis]

[group gitosis-admin]

writable = gitosis-admin

members = jason@mymacbook.local

[group marketsim-team]

members = jason@mymacbook.local

writable = marketsim

[repo marketsim]

gitweb = no

description = Market Simulation App

owner = Jason Miller

daemon = no

```

I could add 'newteammember.pub' in the 'keydir' folder, then add 'newteammember' after 'jason@mymacbook.local' separated by a space. This would make another team member part of that group which has write access to the repository.

After configuring a new repository, and giving my own local user write access to that repository, I've push the changes via a commit to the remote gitosis-admin repository.

NOTE: You may receive the warning "remote: WARNING:gitosis.gitweb.set_descriptions:Cannot find 'yourrepo' in '/home/git/repositories'". Ignore this and continue.

Now I'm going to initialize my new repository and push it to the remote server.

``` shell

$ cd marketsim

$ git init .

$ git add .

$ git commit -m "initial commit"

$ git remote add origin git@vps.web-app-host.com:marketsim.git

$ git push origin master

stdin: is not a tty

Initialized empty Git repository in /home/git/repositories/marketsim.git/

Counting objects: 3, done.

Writing objects: 100% (3/3), 209 bytes, done.

Total 3 (delta 0), reused 0 (delta 0)

To git@vps.web-app-host.com:marketsim.git

 * [new branch]      master -> master

```

## Deploying to Server

I've created an account with the username 'marketsi' to host the deployed application (cPanel only allows up to 8 characters for the username). Then I've logged into that account and added my public key via the SSH/Shell Access > Manage SSH Keys section of the cPanel account. 

For property deployment you'll need to install the 'Bundler' gem so that the deployment script can install the gems needed for your application. You'll need to install this as 'root' so that the 'bundle' script is available under /usr/bin/bundle.

``` shell

$ ssh root@vps.web-app-host.com

root@vps [~]# gem install bundler

Fetching: bundler-1.0.21.gem (100%)

Successfully installed bundler-1.0.21

1 gem installed

Installing ri documentation for bundler-1.0.21...

Installing RDoc documentation for bundler-1.0.21...

root@vps [~]# which bundle

/usr/bin/bundle

```

I've modified my deploy.rb file for my Rails application like so:

<pre class="brush:rails">
require "bundler/capistrano"

load "deploy/assets"

#############################################################

# Settings

set :application, "marketsim"

default_run_options[:pty] = true  # Must be set for the password prompt from git to work

set :use_sudo, false

set :user, "marketsi"  # The server's user for deploys

set :deploy_to, "/home/#{user}/rails"

set :ssh_options, { :forward_agent => true }

set :domain, "marketsim.org"

server domain, :app, :web

role :db, domain, :primary => true

#############################################################

# Git

set :scm, :git

set :repository,  "git@vps.web-app-host.com:marketsim.git"

set :branch, "master"

set :deploy_via, :remote_cache

#############################################################

# Passenger

namespace :passenger do

  desc "Restart Application"

  task :restart do

    run "touch #{current_path}/tmp/restart.txt"

  end

end

after :deploy, "passenger:restart"

```

Now to run the script to setup the deployment directories on the remote server.

``` shell

cap deploy:setup

```

This created a folder under /home/marketsi/rails with the 'releases' and 'shared' folder. Now I'll actually deploy.

``` shell

cap deploy

```

The gems were installed with no issue by Bundler for me. Hopefully the same goes for you.

## Passenger

The next step is to configure Apache to serve the Rails application for my domain name. Install the Passenger gem via SSH logged in as root:

``` shell

gem install passenger

```

Install Passenger using the Apache module installation command. All dependencies should be found and displayed in green.

``` shell

passenger-install-apache2-module

```

At the end of the installation script it provides the Apache configuration settings which you place in httpd.conf. Since we're using cPanel, which over-writes the Apache configuration when the EasyApache system is used to rebuild Apache, PHP, and other modules, place this configuration in /usr/local/apache/conf/includes/pre_main_global.conf.

<pre class="brush:text">
# /usr/local/apache/conf/includes/pre_main_global.conf

LoadModule passenger_module /usr/lib/ruby/gems/1.8/gems/passenger-3.0.11/ext/apache2/mod_passenger.so

PassengerRoot /usr/lib/ruby/gems/1.8/gems/passenger-3.0.11

PassengerRuby /usr/bin/ruby

```

Next make a backup of the httpd.conf, run the configuration distiller script, rebuild, and then restart Apache.

``` shell

cp /usr/local/apache/conf/httpd.conf /usr/local/apache/conf/httpd.conf.bak-modrails

/usr/local/cpanel/bin/apache_conf_distiller --update

/scripts/rebuildhttpdconf

/etc/init.d/httpd restart

```

The cPanel system makes the Apache Document Root for each account map to /home/username/public_html. Because of this you will need to remove the 'public_html' directory, and then create a symlink from that directory to the 'public' directory for your applications current release:

``` shell

rm -rf /home/marketsi/public_html/

ln -s /home/marketsi/rails/current/public /home/marketsi/public_html

chown marketsi:nobody public_html/

chmod 750 public_html/

```

Next add a .htaccess file in your application under the 'public' folder, and make sure it contains 'RailsBaseURI /', as well as a directive with the PassengerAppRoot.

<pre class="brush:rails">
RailsBaseURI /

PassengerAppRoot /home/marketsi/rails/current

```

## MySQL Database

If your application returns an error page 'We're sorry, but something went wrong.', check the production.log file on the server. In my case, the application was running, but it couldn't connect to the database with the existing database.yml settings for production.

As cPanel controls the MySQL databases and usernames, you'll have to create a database manually via the cPanel, create the user and assign all privileges to it for the database, and then configure your database.yml appropriately.

