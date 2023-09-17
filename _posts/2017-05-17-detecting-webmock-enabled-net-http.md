---
layout: post
title: Detecting if WebMock is enabled for Net::HTTP
date: '2017-05-17 14:43:52 -0700'
comments: true
categories:
- Ruby
tags:
- WebMock
- HTTParty
---

I ran into an issue where we were mocking HTTP responses 400+ in our Rspec
tests, which resulted in our application logging an error and a stack trace.
When we expect errors because we're using WebMock to emulate an HTTP 500
response, logging the stack trace involved can be too verbose.
<!--more-->

Sometimes we might need the stack trace, such as when a developer is debugging
code involving the handling of error responses. I discussed this with other
developers they expressed that they don't want to introduce a global
configuration flag to turn the stack trace logging on or off.

The ideal solution was to simply not log the stack trace when WebMock is being
used in the 'test' environment.

We're using HTTParty, which uses Net::HTTP. I did some investigating and
discovered that when WebMock is enabled (via `WebMock.enable!`), that it
replaces the HTTP module with it's own. There isn't anything clear to indicate
if WebMock is enabled or not, however I noticed that
[Net::HTTP.socket_type is redefined as StubSocket] when WebMock is enabled.

```ruby
> Net::HTTP.socket_type
=> Net::BufferedIO
> WebMock.enable!
=> {:net_http=>WebMock::HttpLibAdapters::NetHttpAdapter}
> Net::HTTP.socket_type
=> StubSocket
> WebMock.disable!
> Net::HTTP.socket_type
=> Net::BufferedIO
```

I don't see the equivalent modification in other adapters, such as [Curb].

I'm going to open a Github issue requesting support such as this. Perhaps a
simple unified call to set a class variable in WebMock class itself, with a
corresponding `WebMock.is_enabled?` method. I've made [an issue] to request
this. I will make a pull request soon.

[Net::HTTP.socket_type is redefined as StubSocket]: https://github.com/bblimke/webmock/blob/e3d0cd1c/lib/webmock/http_lib_adapters/net_http.rb#L45,L47
[Curb]: https://github.com/bblimke/webmock/blob/e3d0cd1c/lib/webmock/http_lib_adapters/curb_adapter.rb
[an issue]: https://github.com/bblimke/webmock/issues/701
