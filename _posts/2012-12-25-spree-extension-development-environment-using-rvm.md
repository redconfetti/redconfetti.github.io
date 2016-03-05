---
layout: post
status: publish
published: true
title: Spree Extension Development Environment using RVM
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1400
wordpress_url: http://www.rubycoloredglasses.com/?p=1400
date: '2012-12-25 10:30:38 -0800'
date_gmt: '2012-12-25 10:30:38 -0800'
categories:
- Gems
tags:
- Spree
- extension
comments: []
---
<p>I've found that there is trouble working with a Spree extension when your gem set does not include the gems included with the Spree gem itself. I discovered this after generating a Spree extension, confining the extension to it's own gem set using RVM, and then running 'bundle install' based on the Gemfile/gemspec configuration of just the extension itself.</p>
<p>To overcome this, I recommend making a folder named 'spree', then configuring that folder to use a shared 'Spree' gem set.</p>
<pre class="brush:shell">$ mkdir spree<br />
$ cd spree<br />
$ rvm --rvmrc --create 1.9.3-p327@spree<br />
$ cd ..<br />
$ cd spree<br />
====================================================================================<br />
= NOTICE                                                                           =<br />
====================================================================================<br />
= RVM has encountered a new or modified .rvmrc file in the current directory       =<br />
= This is a shell script and therefore may contain any shell commands.             =<br />
=                                                                                  =<br />
= Examine the contents of this file carefully to be sure the contents are          =<br />
= safe before trusting it! ( Choose v[iew] below to view the contents )            =<br />
====================================================================================<br />
Do you wish to trust this .rvmrc file? (/Users/jason/Sites/spree/.rvmrc)<br />
y[es], n[o], v[iew], c[ancel]> yes</pre><br />
Next, install the Rails and Spree gems, generate a Rails application, install Spree into that application. You will use this application to explore Spree, experiment with it, etc. It's main purpose is to install the gems that are dependencies of the Spree gem.</p>
<pre class="brush:shell">$ gem install rails -v 3.2.9<br />
$ gem install spree -v 1.3.0<br />
$ rails _3.2.9_ new my_store<br />
$ spree install my_store</pre><br />
In the same directory, generate the Spree extension.</p>
<pre class="brush:shell">$ spree extension myextension<br />
      create  spree_myextension<br />
      create  spree_myextension/app<br />
      create  spree_myextension/app/assets/javascripts/admin/spree_myextension.js<br />
      create  spree_myextension/app/assets/javascripts/store/spree_myextension.js<br />
      create  spree_myextension/app/assets/stylesheets/admin/spree_myextension.css<br />
      create  spree_myextension/app/assets/stylesheets/store/spree_myextension.css<br />
      create  spree_myextension/lib<br />
      create  spree_myextension/lib/spree_myextension.rb<br />
      create  spree_myextension/lib/spree_myextension/engine.rb<br />
      create  spree_myextension/lib/generators/spree_myextension/install/install_generator.rb<br />
      create  spree_myextension/script<br />
      create  spree_myextension/script/rails<br />
      create  spree_myextension/spree_myextension.gemspec<br />
      create  spree_myextension/Gemfile<br />
      create  spree_myextension/.gitignore<br />
      create  spree_myextension/LICENSE<br />
      create  spree_myextension/Rakefile<br />
      create  spree_myextension/README.md<br />
      create  spree_myextension/config/routes.rb<br />
      create  spree_myextension/config/locales/en.yml<br />
      create  spree_myextension/.rspec<br />
      create  spree_myextension/spec/spec_helper.rb<br />
      create  spree_myextension/Versionfile</p>
<p>        ********************************************************************************</p>
<p>        Your extension has been generated with a gemspec dependency on Spree 1.3.0.</p>
<p>        Please update the Versionfile to designate compatibility with different versions of Spree.<br />
        See http://spreecommerce.com/documentation/extensions.html#versionfile</p>
<p>        Consider listing your extension in the official extension registry http://spreecommerce.com/extensions"</p>
<p>        ********************************************************************************</pre><br />
Now that the extension is created, you'll need to make a minor modification to the configuration of the extensions Gemfile by forcing it to use the 'edge' branch version of the 'spree_auth_devise' gem. This requires simply adding the ", :branch => 'edge'" to the end of the existing definition in the Gemfile for the extension. This step ensures that you do not receive any errors during the next step.</p>
<pre class="brush:shell">gem 'spree_auth_devise', :git => "git://github.com/spree/spree_auth_devise", :branch => 'edge'</pre><br />
Now you will need to generate the dummy Rails application which is used by your Rspec tests. As you are generating an extension that is meant to integrate with an existing Rails application environment, with the Spree gem installed, this is needed to ensure that you can rely on the Rails and Spree libraries being present during the tests.</p>
<pre class="brush:shell">$ bundle exec rake test_app<br />
Thor has already been required. This may cause Bundler to malfunction in unexpected ways.<br />
Generating dummy Rails application...<br />
Setting up dummy database...</pre><br />
Now, you can troubleshoot issues you run into while developing the extension by dropping into the dummy applications Rails console. The dummy application is located under 'spree_myextension/spec/dummy'. You should keep this application configured so that it's configured as a stock Rails application with Spree installed, with your Rspec tests generating test data in the database on the fly.</p>
<p>At most you should modify this Rails application so that it has the necessary modifications which you have documented for the user to make upon installing your extension, as well as the initializers or other files that your gem has <a href="http://guides.rubyonrails.org/generators.html" target="_blank">generators</a> for.</p>
<p>Lastly you can initialize the new extension to sync up with a Git repository. I recommend using <a href="https://bitbucket.org/" target="_blank">Bitbucket</a> if you're not developing a public extension, as they host unlimited private repositories for teams of up to 5 users.</p>
