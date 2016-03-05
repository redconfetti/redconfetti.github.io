---
layout: post
status: publish
published: true
title: How to 'head' a text file in Ruby
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1731
wordpress_url: http://www.rubycoloredglasses.com/?p=1731
date: '2014-01-30 22:51:32 -0800'
date_gmt: '2014-01-30 22:51:32 -0800'
categories:
- Ruby
tags:
- ruby
- head
---
<p>I wanted to just view the first 20 lines of a 10,000 line CSV file returned by an API in a Ruby on Rails project I'm working on. Here is the chain of Ruby commands I came up with to effectively 'head' the CSV document returned.</p>
<pre class="brush:ruby">>> csv = "first line\nsecond line\nthird line\nfourth line\nfifth line\nsixth line\n"<br />
>> csv.split("\n")[0..3].join("\n")<br />
=> "first line\nsecond line\nthird line\nfourth line"</pre></p>
