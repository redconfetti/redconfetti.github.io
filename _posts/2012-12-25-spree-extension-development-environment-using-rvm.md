---
layout: post
title: Spree Extension Development Environment using RVM
date: '2012-12-25 10:30:38 -0800'
categories:
- Gems
tags:
- Spree
- extension
comments: []
---

I've found that there is trouble working with a Spree extension when your gem
set does not include the gems included with the Spree gem itself. I discovered
this after generating a Spree extension, confining the extension to it's own gem
set using RVM, and then running 'bundle install' based on the Gemfile/gemspec
configuration of just the extension itself.

To overcome this, I recommend making a folder named 'spree', then configuring
that folder to use a shared 'Spree' gem set.
<!--more-->

``` shell
$ mkdir spree
$ cd spree
$ rvm --rvmrc --create 1.9.3-p327@spree
$ cd ..
$ cd spree
====================================================================================
= NOTICE                                                                           =
====================================================================================
= RVM has encountered a new or modified .rvmrc file in the current directory       =
= This is a shell script and therefore may contain any shell commands.             =
=                                                                                  =
= Examine the contents of this file carefully to be sure the contents are          =
= safe before trusting it! ( Choose v[iew] below to view the contents )            =
====================================================================================
Do you wish to trust this .rvmrc file? (/Users/jason/Sites/spree/.rvmrc)
y[es], n[o], v[iew], c[ancel]> yes
```

Next, install the Rails and Spree gems, generate a Rails application, install
Spree into that application. You will use this application to explore Spree,
experiment with it, etc. It's main purpose is to install the gems that are
dependencies of the Spree gem.

```bash
gem install rails -v 3.2.9
gem install spree -v 1.3.0
rails _3.2.9_ new my_store
spree install my_store
```

In the same directory, generate the Spree extension.

```bash
$ spree extension myextension
      create  spree_myextension
      create  spree_myextension/app
      create  spree_myextension/app/assets/javascripts/admin/spree_myextension.js
      create  spree_myextension/app/assets/javascripts/store/spree_myextension.js
      create  spree_myextension/app/assets/stylesheets/admin/spree_myextension.css
      create  spree_myextension/app/assets/stylesheets/store/spree_myextension.css
      create  spree_myextension/lib
      create  spree_myextension/lib/spree_myextension.rb
      create  spree_myextension/lib/spree_myextension/engine.rb
      create  spree_myextension/lib/generators/spree_myextension/install/install_generator.rb
      create  spree_myextension/script
      create  spree_myextension/script/rails
      create  spree_myextension/spree_myextension.gemspec
      create  spree_myextension/Gemfile
      create  spree_myextension/.gitignore
      create  spree_myextension/LICENSE
      create  spree_myextension/Rakefile
      create  spree_myextension/README.md
      create  spree_myextension/config/routes.rb
      create  spree_myextension/config/locales/en.yml
      create  spree_myextension/.rspec
      create  spree_myextension/spec/spec_helper.rb
      create  spree_myextension/Versionfile
        ********************************************************************************
        Your extension has been generated with a gemspec dependency on Spree 1.3.0.
        Please update the Versionfile to designate compatibility with different versions of Spree.
        See http://spreecommerce.com/documentation/extensions.html#versionfile
        Consider listing your extension in the official extension registry http://spreecommerce.com/extensions"
        ********************************************************************************
```

Now that the extension is created, you'll need to make a minor modification to
the configuration of the extensions Gemfile by forcing it to use the 'edge'
branch version of the 'spree_auth_devise' gem. This requires simply adding the
", :branch => 'edge'" to the end of the existing definition in the Gemfile for
the extension. This step ensures that you do not receive any errors during the
next step.

``` shell
gem 'spree_auth_devise', :git => "git://github.com/spree/spree_auth_devise", :branch => 'edge'
```

Now you will need to generate the dummy Rails application which is used by your
Rspec tests. As you are generating an extension that is meant to integrate with
an existing Rails application environment, with the Spree gem installed, this is
needed to ensure that you can rely on the Rails and Spree libraries being
present during the tests.

``` shell
$ bundle exec rake test_app
Thor has already been required. This may cause Bundler to malfunction in unexpected ways.
Generating dummy Rails application...
Setting up dummy database...
```

Now, you can troubleshoot issues you run into while developing the extension by
dropping into the dummy applications Rails console. The dummy application is
located under 'spree_myextension/spec/dummy'. You should keep this application
configured so that it's configured as a stock Rails application with Spree
installed, with your Rspec tests generating test data in the database on the fly.

At most you should modify this Rails application so that it has the necessary
modifications which you have documented for the user to make upon installing
your extension, as well as the initializers or other files that your gem has
[generators] for.

Lastly you can initialize the new extension to sync up with a Git repository. I
recommend using [Bitbucket] if you're not developing a
public extension, as they host unlimited private repositories for teams of up
to 5 users.

[Bitbucket]: https://bitbucket.org/
[generators]: http://guides.rubyonrails.org/generators.html
