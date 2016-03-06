---
layout: post
title: How to 'head' a text file in Ruby
date: '2014-01-30 22:51:32 -0800'
categories:
- Ruby
tags:
- ruby
- head
---
I wanted to just view the first 20 lines of a 10,000 line CSV file returned by an API in a Ruby on Rails project I'm working on. Here is the chain of Ruby commands I came up with to effectively 'head' the CSV document returned.

``` ruby
>> csv = "first line\nsecond line\nthird line\nfourth line\nfifth line\nsixth line\n"
>> csv.split("\n")[0..3].join("\n")
=> "first line\nsecond line\nthird line\nfourth line"
```

