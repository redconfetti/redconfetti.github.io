---
layout: post
status: publish
published: true
title: Rails 3 Autoloading with Namespaced Models
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 167
wordpress_url: http://rails.redconfetti.com/?p=167
date: '2011-10-18 15:50:03 -0700'
date_gmt: '2011-10-18 22:50:03 -0700'
categories:
- Model
tags: []
comments: []
---
<p>I'm working through an upgrade from Rails 2.3.8 to Rails 3.1, and a set of name spaced models that I setup are giving errors when I run a certain Rake task that relies on them. I looked into the issue and it appears that I need to learn the way Rails 3 loads models.</p>
<p>In &#47;config&#47;application.rb I'm using the following line to auto load all models from &#47;app&#47;models and any subdirectories.</p>
<pre class="brush:rails">config.autoload_paths += Dir["#{config.root}&#47;app&#47;models&#47;**&#47;"]<&#47;pre><br />
For this to work I found that I needed to understand how Rails 3 interprets the folder and filenames. Some models weren't even registering as available, so I created folders for each class and put their files under each folder. This seemed to resolve some errors, but then I was receiving errors when I would try to instantiate a new object from one of the defined classes in the Rails console:</p>
<pre class="brush:shell">NoMethodError: undefined method `new' for RetsMap::Sys_Local::Res_property::Class_1:Module<&#47;pre><br />
In my case RetsMap::Sys_Local::Res_property inherits from RetsMap::Base, and RetsMap::Sys_Local::Res_property::Class_1 inherits from RetsMap::Sys_Local::Res_property. I then tried to see why I was receiving this mention of 'Module' at the end of my 'Class_1' by starting with RetsMap::Base.</p>
<pre class="brush:shell">irb(main):001:0> RetsMap.class<br />
=> Module<br />
irb(main):002:0> RetsMap::Base<br />
=> RetsMap::Base<br />
irb(main):003:0> RetsMap::Base.class<br />
=> Module<&#47;pre><br />
Sure enough this showed that Rails was interpreting my class as a module. Since these sub-classes inherit from RetsMap::Base, which is loading as a Module, I figured that I needed to start there. I moved the file which defined the class from &#47;app&#47;models&#47;rets_map&#47;base&#47; folder to &#47;app&#47;models&#47;rets_map&#47;base.rb and this caused the class to load as a class.</p>
<pre class="brush:shell">irb(main):005:0> RetsMap.class<br />
=> Module<br />
irb(main):006:0> RetsMap::Base.class<br />
=> Class<&#47;pre><br />
I take it that this means that the folders are meant to store modules, with the files representing the classes. At the same time, a subdirectory must be created for each subsequent namespace or else you'll receive an error that it was expecting the file to define itself in the namespace of the folder it belongs to. If you create a file in a directory with the same name as the directory, it can define a class for that namespace instead of being loaded as a module.</p>