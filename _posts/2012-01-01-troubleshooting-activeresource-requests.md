---
layout: post
status: publish
published: true
title: Troubleshooting ActiveResource Requests
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 947
wordpress_url: http://www.redconfetti.com/?p=947
date: '2012-01-01 18:46:52 -0800'
date_gmt: '2012-01-01 22:46:52 -0800'
categories:
- Ruby on Rails
tags:
- ActiveResource
- HighRise
- REST API
comments: []
---
<p>I'm currently working on an app which integrates with the <a href="http://developer.37signals.com/highrise/people" target="_blank">HighRise API</a> using the <a href="https://github.com/tapajos/highrise" target="_blank">Highrise Ruby wrapper gem</a>. The classes defined by the gem rely on ActiveResource, the Ruby library included with Rails for interfacing with RESTful resources like the HighRise API.</p>
<p>Sometimes I'm not sure if the requests being made via the commands I'm using are making the right calls on the HighRise API.</p>
<p>I found this thread on StackOverflow: <a href="http://stackoverflow.com/questions/227907/how-do-i-view-the-http-response-to-an-activeresource-request" target="_blank">How do I view the HTTP response to an ActiveResource request?</a>. The solution I found helpful was this one on <a href="http://www.jroller.com/bokmann/entry/debugging_activerecord_web_services" target="_blank">overriding ActiveResource::Connection</a> so that it outputs the debug output. This worked for me even with Rails 3.1.</p>
<p>Just in case this article with the code disappears, here is the initializer code you can use to view the HTTP session data from the console. I've added an if statement to make sure the HTTP data is only outputted to the standard output in development mode, for <a href="http://ruby-doc.org/stdlib-1.9.3/libdoc/net/http/rdoc/Net/HTTP.html#method-i-set_debug_output" target="_blank">security purposes</a>, as well as requires the environment variable 'HTTPDEBUG'.</p>
<pre class="brush:rails"># /config/initializers/connection.rb<br />
class ActiveResource::Connection<br />
  # Creates new Net::HTTP instance for communication with<br />
  # remote service and resources.<br />
  def http<br />
    http = Net::HTTP.new(@site.host, @site.port)<br />
    http.use_ssl = @site.is_a?(URI::HTTPS)<br />
    http.verify_mode = OpenSSL::SSL::VERIFY_NONE if http.use_ssl<br />
    http.read_timeout = @timeout if @timeout<br />
    #Here's the addition that allows you to see the output<br />
    if Rails.env == 'development' &amp;&amp; ENV['HTTPDEBUG']<br />
      http.set_debug_output $stderr<br />
    end<br />
    return http<br />
  end<br />
end</pre><br />
You can open the Rails console using 'HTTPDEBUG=true rails c' to activate the console with debugging output displayed in the console mode.</p>
