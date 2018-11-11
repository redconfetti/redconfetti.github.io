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
---

I'm currently working on an app which integrates with the [HighRise API][1]
using the [Highrise Ruby wrapper gem][2]. The classes defined by the gem rely
on ActiveResource, the Ruby library included with Rails for interfacing with
RESTful resources like the HighRise API.

Sometimes I'm not sure if the requests being made via the commands I'm using
are making the right calls on the HighRise API.

I found this thread on StackOverflow:
[How do I view the HTTP response to an ActiveResource request?][3]. The
solution I found helpful was this one on
[overriding ActiveResource::Connection][4] so that it outputs the debug output.
This worked for me even with Rails 3.1.

Just in case this article with the code disappears, here is the initializer
code you can use to view the HTTP session data from the console. I've added an
if statement to make sure the HTTP data is only outputted to the standard
output in development mode, for [security purposes][5], as well as requires
the environment variable 'HTTPDEBUG'.

``` ruby
# /config/initializers/connection.rb
class ActiveResource::Connection

  # Creates new Net::HTTP instance for communication with
  # remote service and resources.
  def http
    http = Net::HTTP.new(@site.host, @site.port)
    http.use_ssl = @site.is_a?(URI::HTTPS)
    http.verify_mode = OpenSSL::SSL::VERIFY_NONE if http.use_ssl
    http.read_timeout = @timeout if @timeout

    # Here's the addition that allows you to see the output
    if Rails.env == 'development' &amp;&amp; ENV['HTTPDEBUG']
      http.set_debug_output $stderr
    end

    return http
  end
end
```

You can open the Rails console using 'HTTPDEBUG=true rails c' to activate the
console with debugging output displayed in the console mode.

[1]: http://developer.37signals.com/highrise/people
[2]: https://github.com/tapajos/highrise
[3]: http://stackoverflow.com/questions/227907/how-do-i-view-the-http-response-to-an-activeresource-request
[4]: http://www.jroller.com/bokmann/entry/debugging_activerecord_web_services
[5]: http://ruby-doc.org/stdlib-1.9.3/libdoc/net/http/rdoc/Net/HTTP.html#method-i-set_debug_output
