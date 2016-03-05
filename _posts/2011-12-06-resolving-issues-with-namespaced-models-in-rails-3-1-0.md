---
layout: post
status: publish
published: true
title: Resolving issues with Namespaced Models in Rails 3.1.0
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 929
wordpress_url: http://www.redconfetti.com/?p=929
date: '2011-12-06 19:09:51 -0800'
date_gmt: '2011-12-06 23:09:51 -0800'
categories:
- Ruby on Rails
tags:
- rails
- namespaced models
- rails3.1
comments: []
---
<p>I recently was tasked with upgrading an application from Rails 2.3.8 to Rails 3. I choose to upgrade it to Rails 3.1, because why upgrade once and then have to do it again later.</p>
<p>After upgrading and testing many points of the system locally, I was ready to push the upgraded application to the production server. After pushing it out I started to notice that a certain rake task was failing to run via a cronjob I had setup. This rake task worked with certain non-ActiveRecord models, which I had setup with a hierarchy which utilized inheritance and namespacing.</p>
<p>Under Rails 2.3.8 I had a file, /app/models/rets_map/rets_map_base.rb. The error I was receiving from the rake task involved some sort of issue loading the class from this file.</p>
<p>As an experiment I renamed the file as 'base.rb' so that it's path was /app/models/rets_map/base.rb. I figured that Rails 3.1.0 expected this naming convention. I was right, kind of...</p>
<pre class="brush:shell">$ rails console<br />
/Users/jason/Sites/rj4/rails/app/controllers/site_setup_controller.rb:120: warning: string literal in condition<br />
Loading development environment (Rails 3.1.0)<br />
irb(main):001:0> RetsMap<br />
=> RetsMap<br />
irb(main):002:0> RetsMap::Base<br />
=> RetsMap::Base<br />
irb(main):003:0> reload!<br />
Reloading...<br />
=> true<br />
irb(main):004:0> RetsMap<br />
=> RetsMap<br />
irb(main):005:0> RetsMap::Base<br />
=> RetsMap::Base<br />
irb(main):006:0> reload!<br />
Reloading...<br />
=> true<br />
irb(main):007:0> RetsMap<br />
=> RetsMap<br />
irb(main):008:0> RetsMap::Base<br />
LoadError: Expected /Users/jason/Sites/rj4/rails/app/models/rets_map/base.rb to define RetsMap::Base<br />
	from /opt/local/lib/ruby/gems/1.8/gems/activesupport-3.1.0/lib/active_support/dependencies.rb:490:in `load_missing_constant'<br />
	from /opt/local/lib/ruby/gems/1.8/gems/activesupport-3.1.0/lib/active_support/dependencies.rb:181:in `const_missing'<br />
	from /opt/local/lib/ruby/gems/1.8/gems/activesupport-3.1.0/lib/active_support/dependencies.rb:179:in `each'<br />
	from /opt/local/lib/ruby/gems/1.8/gems/activesupport-3.1.0/lib/active_support/dependencies.rb:179:in `const_missing'<br />
	from (irb):8</pre><br />
For some reason after I would tell the console to reload the models, I got an error that the file didn't define RetsMap::Base....but it did damnit!!</p>
<p>The error was regarding ActiveSupport, so I tried to reinstall the Rails gem and ActiveSupport gem, but this didn't help.</p>
<p>The solution I had found was to upgrade to Rails 3.1.1. After doing this, the error no longer occurred, even after I had reloaded the models.</p>
<p>Ooh, also, I didn't mention. Previously I was using the following command in /config/application.rb:</p>
<pre class="brush:rails">config.autoload_paths += Dir["#{config.root}/app/models/**/"]</pre><br />
As part of my troubleshooting I figured that by default Rails will try to load models from subdirectories if they conform to the proper directory and file naming required for namespaced models. When you add models to the autoload paths configuration, I think it expects that those conventions don't apply...or maybe this just causes problems. I commented this line out and simply added each subdirectory that contained non-namespaced models.</p>
<pre class="brush:rails">config.autoload_paths += %W(#{config.root}/app/models/email/)<br />
config.autoload_paths += %W(#{config.root}/app/models/sub/)<br />
config.autoload_paths += %W(#{config.root}/app/models/upload/)</pre></p>
