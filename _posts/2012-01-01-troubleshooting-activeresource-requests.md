---
layout: post
title: Troubleshooting ActiveResource Requests
date: '2012-01-01 18:46:52 -0800'
categories:
- Ruby on Rails
tags:
- ActiveResource
- HighRise
- REST API
comments: []
---
I'm currently working on an app which integrates with the <a href="http://developer.37signals.com/highrise/people" target="_blank">HighRise API</a> using the <a href="https://github.com/tapajos/highrise" target="_blank">Highrise Ruby wrapper gem</a>. The classes defined by the gem rely on ActiveResource, the Ruby library included with Rails for interfacing with RESTful resources like the HighRise API.

Sometimes I'm not sure if the requests being made via the commands I'm using are making the right calls on the HighRise API.

I found this thread on StackOverflow: <a href="http://stackoverflow.com/questions/227907/how-do-i-view-the-http-response-to-an-activeresource-request" target="_blank">How do I view the HTTP response to an ActiveResource request?</a>. The solution I found helpful was this one on <a href="http://www.jroller.com/bokmann/entry/debugging_activerecord_web_services" target="_blank">overriding ActiveResource::Connection</a> so that it outputs the debug output. This worked for me even with Rails 3.1.

Just in case this article with the code disappears, here is the initializer code you can use to view the HTTP session data from the console. I've added an if statement to make sure the HTTP data is only outputted to the standard output in development mode, for <a href="http://ruby-doc.org/stdlib-1.9.3/libdoc/net/http/rdoc/Net/HTTP.html#method-i-set_debug_output" target="_blank">security purposes</a>, as well as requires the environment variable 'HTTPDEBUG'.

<pre class="brush:rails"># /config/initializers/connection.rb

class ActiveResource::Connection

  # Creates new Net::HTTP instance for communication with

  # remote service and resources.

  def http

    http = Net::HTTP.new(@site.host, @site.port)

    http.use_ssl = @site.is_a?(URI::HTTPS)

    http.verify_mode = OpenSSL::SSL::VERIFY_NONE if http.use_ssl

    http.read_timeout = @timeout if @timeout

    #Here's the addition that allows you to see the output

    if Rails.env == 'development' &amp;&amp; ENV['HTTPDEBUG']

      http.set_debug_output $stderr

    end

    return http

  end

end```

You can open the Rails console using 'HTTPDEBUG=true rails c' to activate the console with debugging output displayed in the console mode.

