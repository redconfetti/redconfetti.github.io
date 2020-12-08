---
layout: post
title: Setting up Ubuntu for Rails App via Passenger
date: '2011-10-29 22:11:11 -0700'
comments: true
categories:
- Hosting
---

These instructions apply to Ubuntu 11.04 (natty).

Run the following to install Ruby, the Apache2 web server, Curl Development
Headers with SSL Support.

``` shell
sudo apt-get update
sudo apt-get install build-essential
sudo apt-get install ruby ruby1.8-dev
sudo apt-get install apache2 apache2-mpm-prefork apache2-prefork-dev libcurl4-openssl-dev
```

<!--more-->

Next download Ruby Gems, extract the package, install Ruby Gems, and then
create a symbolic link from /usr/bin/gem1.8 to /usr/bin/gem

``` shell
cd ~
wget http://rubyforge.org/frs/download.php/75309/rubygems-1.8.10.tgz
tar -zxvf rubygems-1.8.10.tgz
cd rubygems-1.8.10
sudo ruby setup.rb
ln -s /usr/bin/gem1.8 /usr/bin/gem
```

With Ruby Gems installed we can install the Passenger gem.

``` shell
sudo gem install passenger
```

Now we can run the installation of Passenger for Apache2.

``` shell
passenger-install-apache2-module
```

At the end of the build the installation instructions tell you to add
something like the following to your Apache configuration file.

``` shell
LoadModule passenger_module /usr/lib/ruby/gems/1.8/gems/passenger-3.0.9/ext/apache2/mod_passenger.so
PassengerRoot /usr/lib/ruby/gems/1.8/gems/passenger-3.0.9
PassengerRuby /usr/bin/ruby1.8
```

Do this by copying the commands, then pasting them into /etc/apache2/mods
available/passenger.load

``` shell
sudo nano /etc/apache2/mods-available/passenger.load
```

After saving the file, run the following commands to load the module and
restart the Apache web server.

``` shell
a2enmod passenger
/etc/init.d/apache2 restart
```

Now you can create a virtual host file for your site under
/etc/apache2/sites-available

``` shell
sudo nano /etc/apache2/sites-available/yoursite.com
```

Then setup this virtualhost file to load and tell Apache to reload the
configuration

``` shell
a2ensite yoursite.com
/etc/init.d/apache2 reload
```

For your application to run you'll need to install the Rails gem of course.

``` shell
sudo gem install rails
```
