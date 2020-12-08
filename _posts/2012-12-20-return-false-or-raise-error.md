---
layout: post
title: Return FALSE or Raise Error?
date: '2012-12-20 18:35:44 -0800'
comments: true
categories:
- Ruby
tags:
- ruby
- exception handling
comments: []
---

I was working on a gem a couple months ago, and it came time for my boss to do
a code review before we install the gem on another teams system. My boss pointed
out that there were areas where I was returning FALSE, and baking in a lot of
conditional statements and other handling, instead of using Ruby's built in
feature of error handling, which is designed to bubble exceptions up the call
stack. He informed me that in situation that are not expected to occur, it's best
to raise an exception to halt execution and report the issue. He even
recommended that I read [Exceptional Ruby], a book devoted to the subject of
proper exception handling.

[Exceptional Ruby]: http://exceptionalruby.com/
<!--more-->

I didn't understand that Ruby exceptions bubble up to the previously calling
scripts, and thus can be captured and handled further up the stack. My boss
pointed out that since my gem would be used only by a special controller setup
on this separate system, in the service of providing an API, I could incorporate
exception handling at the controller level which would handle different types of
errors.

We ended up defining several custom exception classes, used for different types
of error which might occur. For instance, errors resulting from unexpected API
calls would raise the custom MyAPI::UsageError (which inherits from standard
RuntimeError), while expectations placed on their system while interfacing with
its classes would raise one of the standard errors.

Part of the Exceptional Ruby guide informed me that you don't always have to use
a begin..rescue..end block. You can simply insert a rescue block at the end a
method, thus rescuing all statements before it. As you can see, we simply
rescued the types of exceptions caused by the system calling the API so that it
would simply return the error in the response, instead of raising the error in
the remote systems [Airbrake] logs.

```ruby
def handler
  raise MyApi::UsageError, "Request must be HTTP POST" unless request.post?
  @response = Kabam::GmoApi::Server.handler(params[:json_request])
  render :text => @response.to_json
rescue MyApi::InvalidInputError, MyApi::UsageError => e
  error_response = MyApi::Response.new(:status => 'failure', :result => e.message)
  render :text => error_response.to_json
end
```

For reference, here is the hierarchy of standard Ruby exceptions which you can
use, or inherit from for your own custom exception classes.

```shell
Exception
 NoMemoryError
 ScriptError
   LoadError
   NotImplementedError
   SyntaxError
 SignalException
   Interrupt
 StandardError
   ArgumentError
   IOError
     EOFError
   IndexError
   LocalJumpError
   NameError
     NoMethodError
   RangeError
     FloatDomainError
   RegexpError
   RuntimeError
   SecurityError
   SystemCallError
   SystemStackError
   ThreadError
   TypeError
   ZeroDivisionError
 SystemExit
 fatal
```

[airbrake]: http://airbrake.io
