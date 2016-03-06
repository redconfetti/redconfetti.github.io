---
layout: post
title: Ruby on Rails session - Access from PHP
date: '2010-12-14 00:25:37 -0800'
categories:
- Ruby on Rails
tags: []
comments: []
---
If you need to access a Ruby on Rails session from a PHP application running under the same domain, you can do this by splitting the string in the cookie by the '--'. Thanks to Frederick Cheung for <a href="http://www.ruby-forum.com/topic/158813">pointing this out</a>.

Here is coding I added to my PHP script which was running from a path under the same domain. Unfortunately the data returned is in Marshal format, and there isn't a Marshal.load function for PHP to get the values easily.

``` php
$raw_session_string = $_COOKIE['_app_session'];
$data_split = explode ('--' , $raw_session_string);
$encoded_session = $data_split[0];
$marshal_data = base64_decode($encoded_session);
echo "<pre>marshal_data:". print_r($marshal_data,1) ."</pre>\n";
```
