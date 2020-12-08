---
layout: post
title: Configuring Rails 3.1.3 under Sub-URI
date: '2012-01-04 02:02:40 -0800'
categories:
- Ruby on Rails
tags:
- rails3.1
- relative_url_root
- sub-uri
---

In setting up a new Rails app recently I was told that it needed to be served
under the sub-URI of '/info'. I hadn't done this before with a Rails app, and
I expected that it could be tricky.

I checked online to see how this is done and found references to the
'relative_url_root' setting, but then shortly found that this has been
deprecated.

I'm using Phusion Passenger with Apache 2, so I inserted the following
configuration into my VirtualHost entry for the site.
<!--more-->

```apache
Alias /info /home/myapp/current/public

<Location /info>
  PassengerAppRoot /home/myapp/current
  RailsEnv production
</Location>
```

The Rails application was expecting requests from the root of the site still.
I found a StackOverflow article where someone suggested configuring all the
routes under the "/info" scope like so:

``` ruby
# config/routes.rb

scope "/info" do
  root :to => "welcome#index"
  resources :posts
end
```

This worked fine in development mode. I just made sure to pull up the pages
using [http://localhost:3000/info/](http://localhost:3000/info/)

I had tested this in production before building out the application further,
and it seemed to work fine. Later on when I deployed after many updates, I
found that the application was referencing precompiled assets using '/assets/'
and not '/info/assets/'. Further investigation pointed out that I needed to
configure assets with a prefix like so:

``` ruby
# config/application.rb
config.assets.prefix = "/info/assets"
```

This seemed like it would help, but this caused assets to be precompiled under
'public/info/assets'.

I had previously Googled for 'rails 3.1 subdirectory path', not registering
that the more appropriate keyword is 'sub URI' for what I'm trying to
accomplish.

It turns out that the best way to do this with Passenger is to mount the
application to the sub-URI using the proper Apache configuration, and leave it
up to the Rack interface to handle serving the requests to the Rails
application, without using any Rails application configuration for the sub-URI.

I removed the scoped routes, and the assets prefix, returning my application
to it's default configuration. I then created a symbolic link for 'info' in
the websites DocumentRoot, pointing to the 'public' folder of my application
as per the [Passenger documentation][1].

``` shell
ln -s /home/myapp/current/public /home/mysite/public_html/info
```

I added the appropriate configuration into my VirtualHost entry as advised by
the Passenger documentation, but this only worked for the root of the website,
not my admin area I had wanted to serve from /info/admin/. All I got was a 404
page from the main site.

I found that the following configuration worked perfectly, with the symbolic
link still needing to be present.

```apache
Alias /info /home/myapp/current/public

<Location /info>
  PassengerAppRoot /home/myapp/current
  RackEnv production
  RackBaseURI /info
</Location>
```

You might notice that I'm using 'RackEnv production' instead of
'RailsEnv production'. This is because I recall at some point previously that
Rails 3.0 applications are Rack Applications, and thus you have to use
'RackEnv' instead for Passenger to load the application in the proper
environment mode.

----

Update 01/04/12: I'm using the [TinyMCE-Rails plugin][2] to provide a WYSIWYG
editor for one of the resources in my application. It's not loading in
production however (surprise, surprise). I checked into the precompiled
Javascript file being served from the live server, and I see it includes the
following:

``` javascript
window.tinyMCEPreInit=window.tinyMCEPreInit||{base:"/assets/tinymce",query:"3.4.7",suffix:""}
```

The 'base' value should be '/info/assets/tinymce'. I see that in the
TinyMCE-Rails gem the coding for this is:

``` ruby
window.tinyMCEPreInit = window.tinyMCEPreInit || {
  base:   '<%= Rails.application.config.assets.prefix %>/tinymce',
  query:  '<%= TinyMCE::VERSION %>',
  suffix: ''
};
```

All call to 'asset_path('rails.png')' in production returns
'/info/assets/rails-e4b51606cd77fda2615e7439907bfc92.png' so Rails is honoring
the RackSubURI setting configured for Passenger. The following returns the
same path.

The reason why this works is that the Rails asset helpers pull the
relative_url_root value from the controller configuration, which must receive
it from the Rack configuration. When deploying the app using Capistrano, such
configuration is not present, and thus the correct path cannot be included in
the precompiled assets.

The issue was reported via Git hub in issue [#3259][3] and [#2435][4], and a
fix will was implemented in Rails 3.2, with a possibly backport to Rails
3.1.4.

It appears that you'll need to add a configuration to your application.rb or
environments/production.rb, depending on how your development and production
environments are setup.

``` ruby
# config/environments/production.rb

# Use sub-uri in production
config.relative_url_root = '/info'
```

Or possibly configure your deployment script to include the environment
variable when precompiling.

``` shell
RAILS_RELATIVE_URL_ROOT="/info" bundle exec rake assets:precompile
```

I wanted to test this, but I can't seem to update to Rails 3.2, or install a
new Rails app using edge Rails.

```bash
$ rails new testapp --edge
.
.
.
Bundler could not find compatible versions for gem "actionpack":
  In Gemfile:
    sass-rails (>= 0) ruby depends on
      actionpack (~> 3.1.0) ruby
    rails (>= 0) ruby depends on
      actionpack (4.0.0.beta)
```

I'm just going to configure my app under a subdomain instead of sub-uri for
now.

[1]: http://www.modrails.com/documentation/Users%20guide%20Apache.html#deploying_rack_to_sub_uri
[2]: https://github.com/spohlenz/tinymce-rails
[3]: https://github.com/rails/rails/issues/3259
[4]: https://github.com/rails/rails/issues/2435
