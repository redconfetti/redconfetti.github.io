---
layout: post
title: Ruby Class Name
date: '2014-03-20 21:32:23 -0700'
categories:
- Ruby
tags: []
---
I  noticed that in a module used on the CalCentral project that logger expressions used in a module <a href="https://github.com/ets-berkeley-edu/calcentral/blob/cf1af27e53367c24e7769a8655d7014286f08ed0/lib/cache/cacheable.rb#L33" target="_blank">referenced 'self.name'</a> many times. I checked <a href="http://apidock.com/ruby" target="_blank">ApiDock.com</a> for a reference to this class in the Ruby or Rails documentation, but I couldn't find one. The module itself didn't define a #name method, so I was perplexed.

The module I was inspecting is meant to be used to extend other classes, meaning that it establishes the methods as class methods. It turns out that the '<a href="http://www.ruby-doc.org/core-2.1.1/Module.html#method-i-name" target="_blank">Class</a>' class is officially documented as having a #name method that returns a string version of the class name. This is a valid way of logging which class the log message originates from.

