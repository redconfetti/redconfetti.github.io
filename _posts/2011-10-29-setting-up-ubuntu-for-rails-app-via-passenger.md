---
layout: post
status: publish
published: true
title: Setting up Ubuntu for Rails App via Passenger
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 266
wordpress_url: http://rails.redconfetti.com/?p=266
date: '2011-10-29 22:11:11 -0700'
date_gmt: '2011-10-30 05:11:11 -0700'
categories:
- Hosting
tags: []
comments: []
---
<p>These instructions apply to Ubuntu 11.04 (natty).</p>
<p>Run the following to install Ruby, the Apache2 web server, Curl Development Headers with SSL Support.</p>
<pre class="brush:shell">sudo apt-get update<br />
sudo apt-get install build-essential<br />
sudo apt-get install ruby ruby1.8-dev<br />
sudo apt-get install apache2 apache2-mpm-prefork apache2-prefork-dev libcurl4-openssl-dev</pre><br />
Next download Ruby Gems, extract the package, install Ruby Gems, and then create a symbolic link from /usr/bin/gem1.8 to /usr/bin/gem</p>
<pre class="brush:shell">cd ~<br />
wget http://rubyforge.org/frs/download.php/75309/rubygems-1.8.10.tgz<br />
tar -zxvf rubygems-1.8.10.tgz<br />
cd rubygems-1.8.10<br />
sudo ruby setup.rb<br />
ln -s /usr/bin/gem1.8 /usr/bin/gem</pre><br />
With Ruby Gems installed we can install the Passenger gem.</p>
<pre class="brush:shell">sudo gem install passenger</pre><br />
Now we can run the installation of Passenger for Apache2.</p>
<pre class="brush:shell">passenger-install-apache2-module</pre><br />
At the end of the build the installation instructions tell you to add something like the following to your Apache configuration file.</p>
<pre class="brush:shell">LoadModule passenger_module /usr/lib/ruby/gems/1.8/gems/passenger-3.0.9/ext/apache2/mod_passenger.so<br />
PassengerRoot /usr/lib/ruby/gems/1.8/gems/passenger-3.0.9<br />
PassengerRuby /usr/bin/ruby1.8</pre><br />
Do this by copying the commands, then pasting them into /etc/apache2/mods-available/passenger.load</p>
<pre class="brush:shell">sudo nano /etc/apache2/mods-available/passenger.load</pre><br />
After saving the file, run the following commands to load the module and restart the Apache web server.</p>
<pre class="brush:shell">a2enmod passenger<br />
/etc/init.d/apache2 restart</pre><br />
Now you can create a virtual host file for your site under /etc/apache2/sites-available</p>
<pre class="brush:shell">sudo nano /etc/apache2/sites-available/yoursite.com</pre><br />
Then setup this virtualhost file to load and tell Apache to reload the configuration</p>
<pre class="brush:shell">a2ensite yoursite.com<br />
/etc/init.d/apache2 reload</pre><br />
For your application to run you'll need to install the Rails gem of course.</p>
<pre class="brush:shell">sudo gem install rails</pre></p>
