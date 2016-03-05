---
layout: post
status: publish
published: true
title: Using Super with Ruby class methods
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1320
wordpress_url: http://www.rubycoloredglasses.com/?p=1320
date: '2012-09-18 21:06:32 -0700'
date_gmt: '2012-09-18 21:06:32 -0700'
categories:
- Ruby
tags:
- superclass
comments: []
---
<p>One of the awesome things about Ruby is that you can over-ride methods you define, or even over-write methods that are built into Ruby.</p>
<p>This may not be unique with Ruby, but you can also over-ride super class methods in your defined subclass and use 'super' to execute the logic defined in the super class version of that method.</p>
<pre class="brush:rails">
class ScumbagSteve<br />
  def hello<br />
    puts "Hey, can I borrow $5."<br />
  end<br />
end</p>
<p>class GoodGuyGreg < ScumbagSteve<br />
  def hello<br />
    super<br />
    puts "...I'll pay you back tomorrow with interest."<br />
  end<br />
end</p>
<p>> guy = GoodGuyGreg.new<br />
 => #<GoodGuyGreg:0x100346e70><br />
> guy.hello<br />
Hey, can I borrow $5.<br />
...I'll pay you back tomorrow with interest.<br />
 => nil<br />
</pre></p>
