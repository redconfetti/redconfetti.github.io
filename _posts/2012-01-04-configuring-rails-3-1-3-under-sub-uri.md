---
layout: post
status: publish
published: true
title: Configuring Rails 3.1.3 under Sub-URI
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 969
wordpress_url: http://www.redconfetti.com/?p=969
date: '2012-01-04 02:02:40 -0800'
date_gmt: '2012-01-04 06:02:40 -0800'
categories:
- Ruby on Rails
tags:
- rails3.1
- relative_url_root
- sub-uri
comments: []
---
<p>In setting up a new Rails app recently I was told that it needed to be served under the sub-URI of '/info'. I hadn't done this before with a Rails app, and I expected that it could be tricky.</p>
<p>I checked online to see how this is done and found references to the 'relative_url_root' setting, but then shortly found that this has been deprecated.</p>
<p>I'm using Phusion Passenger with Apache 2, so I inserted the following configuration into my VirtualHost entry for the site.</p>
<pre class="brush:text">
Alias /info /home/myapp/current/public<br />
<Location /info><br />
  PassengerAppRoot /home/myapp/current<br />
  RailsEnv production<br />
</Location><br />
</pre></p>
<p>The Rails application was expecting requests from the root of the site still. I found a StackOverflow article where someone suggested configuring all the routes under the "/info" scope like so:</p>
<pre class="brush:rails">
# config/routes.rb<br />
scope "/info" do<br />
  root :to => "welcome#index"<br />
  resources :posts<br />
end<br />
</pre></p>
<p>This worked fine in development mode. I just made sure to pull up the pages using http://localhost:3000/info/</p>
<p>I had tested this in production before building out the application further, and it seemed to work fine. Later on when I deployed after many updates, I found that the application was referencing precompiled assets using '/assets/' and not '/info/assets/'. Further investigation pointed out that I needed to configure assets with a prefix like so:</p>
<pre class="brush:rails">
# config/application.rb<br />
config.assets.prefix = "/info/assets"<br />
</pre></p>
<p>This seemed like it would help, but this caused assets to be precompiled under 'public/info/assets'.</p>
<p>I had previously Googled for 'rails 3.1 subdirectory path', not registering that the more appropriate keyword is 'sub URI' for what I'm trying to accomplish.</p>
<p>It turns out that the best way to do this with Passenger is to mount the application to the sub-URI using the proper Apache configuration, and leave it up to the Rack interface to handle serving the requests to the Rails application, without using any Rails application configuration for the sub-URI.</p>
<p>I removed the scoped routes, and the assets prefix, returning my application to it's default configuration. I then created a symbolic link for 'info' in the websites DocumentRoot, pointing to the 'public' folder of my application as per the <a href="http://www.modrails.com/documentation/Users%20guide%20Apache.html#deploying_rack_to_sub_uri" target="_blank">Passenger documentation</a>.</p>
<pre class="brush:shell">
ln -s /home/myapp/current/public /home/mysite/public_html/info<br />
</pre></p>
<p>I added the appropriate configuration into my VirtualHost entry as advised by the Passenger documentation, but this only worked for the root of the website, not my admin area I had wanted to serve from /info/admin/. All I got was a 404 page from the main site.</p>
<p>I found that the following configuration worked perfectly, with the symbolic link still needing to be present.</p>
<pre class="brush:text">
Alias /info /home/myapp/current/public<br />
<Location /info><br />
  PassengerAppRoot /home/myapp/current<br />
  RackEnv production<br />
  RackBaseURI /info<br />
</Location><br />
</pre></p>
<p>You might notice that I'm using 'RackEnv production' instead of 'RailsEnv production'. This is because I recall at some point previously that Rails 3.0 applications are Rack Applications, and thus you have to use 'RackEnv' instead for Passenger to load the application in the proper environment mode.</p>
<hr />
<p>Update 01/04/12: I'm using the <a href="https://github.com/spohlenz/tinymce-rails" target="_blank">TinyMCE-Rails plugin</a> to provide a WYSIWYG editor for one of the resources in my application. It's not loading in production however (surprise, surprise). I checked into the precompiled Javascript file being served from the live server, and I see it includes the following:</p>
<pre class="brush:javascript">
window.tinyMCEPreInit=window.tinyMCEPreInit||{base:"/assets/tinymce",query:"3.4.7",suffix:""}<br />
</pre></p>
<p>The 'base' value should be '/info/assets/tinymce'. I see that in the TinyMCE-Rails gem the coding for this is:</p>
<pre class="brush:rails">
window.tinyMCEPreInit = window.tinyMCEPreInit || {<br />
  base:   '<%= Rails.application.config.assets.prefix %>/tinymce',<br />
  query:  '<%= TinyMCE::VERSION %>',<br />
  suffix: ''<br />
};<br />
</pre></p>
<p>All call to 'asset_path('rails.png')' in production returns '/info/assets/rails-e4b51606cd77fda2615e7439907bfc92.png' so Rails is honoring the RackSubURI setting configured for Passenger. The following returns the same path.</p>
<p>The reason why this works is that the Rails asset helpers pull the relative_url_root value from the controller configuration, which must receive it from the Rack configuration. When deploying the app using Capistrano, such configuration is not present, and thus the correct path cannot be included in the precompiled assets.</p>
<p>The issue was reported via Git hub in issue #<a href="https://github.com/rails/rails/issues/3259" target="_blank">3259</a> and #<a href="https://github.com/rails/rails/issues/2435" target="_blank">2435</a>, and a fix will was implemented in Rails 3.2, with a possibly backport to Rails 3.1.4. </p>
<p>It appears that you'll need to add a configuration to your application.rb or environments/production.rb, depending on how your development and production environments are setup.</p>
<pre class="brush:rails">
# config/environments/production.rb</p>
<p># Use sub-uri in production<br />
config.relative_url_root = '/info'<br />
</pre></p>
<p>Or possibly configure your deployment script to include the environment variable when precompiling.</p>
<pre class="brush:shell">
RAILS_RELATIVE_URL_ROOT="/info" bundle exec rake assets:precompile<br />
</pre></p>
<p>I wanted to test this, but I can't seem to update to Rails 3.2, or install a new Rails app using edge Rails.</p>
<pre class="brush:shell">
rails new testapp --edge<br />
.<br />
.<br />
.<br />
Bundler could not find compatible versions for gem "actionpack":<br />
  In Gemfile:<br />
    sass-rails (>= 0) ruby depends on<br />
      actionpack (~> 3.1.0) ruby</p>
<p>    rails (>= 0) ruby depends on<br />
      actionpack (4.0.0.beta)<br />
</pre></p>
<p>I'm just going to configure my app under a subdomain instead of sub-uri for now.</p>
