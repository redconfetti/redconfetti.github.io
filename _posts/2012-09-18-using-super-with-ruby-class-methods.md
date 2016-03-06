---
layout: post
title: Using Super with Ruby class methods
date: '2012-09-18 21:06:32 -0700'
categories:
- Ruby
tags:
- superclass
comments: []
---
One of the awesome things about Ruby is that you can over-ride methods you define, or even over-write methods that are built into Ruby.

This may not be unique with Ruby, but you can also over-ride super class methods in your defined subclass and use 'super' to execute the logic defined in the super class version of that method.

``` ruby
class ScumbagSteve
  def hello
    puts "Hey, can I borrow $5."
  end
end

class GoodGuyGreg < ScumbagSteve
  def hello
    super
    puts "...I'll pay you back tomorrow with interest."
  end
end

>> guy = GoodGuyGreg.new
=> #<GoodGuyGreg:0x100346e70>

>> guy.hello
Hey, can I borrow $5.
...I'll pay you back tomorrow with interest.
=> nil
```
