---
layout: post
title: Using URL Helpers in Models or Rake Tasks
date: '2011-10-19 16:41:51 -0700'
categories:
- Model
tags: []
comments: []
---
If for some reason you need to use URL helpers which are based on the routes you've defined in Rails 3.1, simply add the following to the model method or rake task:

``` ruby
include Appname::Application.routes.url_helpers
```

Make sure you replace 'Appname' with the name of your app, which should be the same name as the root folder for your application. You can also obtain it from /config/application.rb where it is defined like so:

``` ruby
module Appname
  class Application < Rails::Application
  end
end
```

If you're needing to render a partial inside of a rake task with Rails 3, you could try using the solution suggested in <a href="http://jguimont.com/post/5582583230/how-to-render-a-full-page-template-in-a-rake-task-with" target="_blank">this article</a>. I had to do this and I pulled it off by adding the include above to the OfflineTemplate class, and then using this code:

``` ruby
Appname::Application.routes.default_url_options = { :host => 'www.mydomain.com' }

template = OfflineTemplate.new

partial_results_html = template.render_to_string(:partial=>"shared/search_results_email_html", :object => search_object, :format => :html)
```

