---
layout: post
status: publish
published: true
title: Ruby on Rails session - Access from PHP
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 653
wordpress_url: http://www.redconfetti.com/?p=653
date: '2010-12-14 00:25:37 -0800'
date_gmt: '2010-12-14 04:25:37 -0800'
categories:
- Ruby on Rails
tags: []
comments: []
---
<p>If you need to access a Ruby on Rails session from a PHP application running under the same domain, you can do this by splitting the string in the cookie by the '--'. Thanks to Frederick Cheung for <a href="http://www.ruby-forum.com/topic/158813">pointing this out</a>.</p>
<p>Here is coding I added to my PHP script which was running from a path under the same domain. Unfortunately the data returned is in Marshal format, and there isn't a Marshal.load function for PHP to get the values easily.</p>
<pre class="brush:php">
$raw_session_string = $_COOKIE['_app_session'];<br />
$data_split = explode ('--' , $raw_session_string);<br />
$encoded_session = $data_split[0];<br />
$marshal_data = base64_decode($encoded_session);<br />
echo "
<pre>marshal_data:". print_r($marshal_data,1) ."</pre>n";<br />
</pre></p>
