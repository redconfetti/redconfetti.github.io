---
layout: post
status: publish
published: true
title: Using URL Helpers in Models or Rake Tasks
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 179
wordpress_url: http://rails.redconfetti.com/?p=179
date: '2011-10-19 16:41:51 -0700'
date_gmt: '2011-10-19 23:41:51 -0700'
categories:
- Model
tags: []
comments: []
---
<p>If for some reason you need to use URL helpers which are based on the routes you've defined in Rails 3.1, simply add the following to the model method or rake task:</p>
<pre class="brush:rails">
include Appname::Application.routes.url_helpers<br />
</pre></p>
<p>Make sure you replace 'Appname' with the name of your app, which should be the same name as the root folder for your application. You can also obtain it from /config/application.rb where it is defined like so:</p>
<pre class="brush:rails">
module Appname<br />
  class Application < Rails::Application<br />
</pre></p>
<p>If you're needing to render a partial inside of a rake task with Rails 3, you could try using the solution suggested in <a href="http://jguimont.com/post/5582583230/how-to-render-a-full-page-template-in-a-rake-task-with" target="_blank">this article</a>. I had to do this and I pulled it off by adding the include above to the OfflineTemplate class, and then using this code:</p>
<pre class="brush:rails">
Appname::Application.routes.default_url_options = { :host => 'www.mydomain.com' }<br />
template = OfflineTemplate.new<br />
partial_results_html = template.render_to_string(:partial=>"shared/search_results_email_html", :object => search_object, :format => :html)<br />
</pre></p>
