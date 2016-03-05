---
layout: post
status: publish
published: true
title: Metaclass
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1324
wordpress_url: http://www.rubycoloredglasses.com/?p=1324
date: '2012-09-19 19:19:03 -0700'
date_gmt: '2012-09-19 19:19:03 -0700'
categories:
- Ruby
tags:
- metaprogramming
- metaclass
comments: []
---
<p>I ran into an instance of meta programming in Ruby today, in the <a href="http://exceptionalruby.com/" target="_blank">Exceptional Ruby</a> book I'm reading for work. It seems that the theme this week is "you don't know Ruby as well as you could".</p>
<p>I might be wrong in my understanding here, but this is what I understand thus far:</p>
<p>Ruby stores methods for an object in it's class, not the object itself. Objects only really store their attributes/variables in memory. However there exists some unseen entity known as the metaclass which belongs to each object, and it can possibly store methods which belong to that object, but not necessarily to that objects class.</p>
<pre class="brush:rails">class Person<br />
  def speak<br />
    puts "Hello There!"<br />
  end<br />
end</p>
<p>john = Person.new<br />
bob = Person.new</p>
<p>class << john<br />
  def bark<br />
    puts "Ruff! Ruff!"<br />
  end<br />
end</p>
<p>> john.speak<br />
Hello There!</p>
<p>> bob.speak<br />
Hello There!</p>
<p>> john.bark<br />
Ruff! Ruff!</p>
<p>> bob.bark<br />
NoMethodError: undefined method `bark' for #<br />
</pre></p>
<p>The reference to 'class << john' opens a code block where methods are defined in the metaclass for 'john', and not the 'Person' class.</p>
<p>A more thorough understanding of this is explored in this blog post - <a href="http://yehudakatz.com/2009/11/15/metaprogramming-in-ruby-its-all-about-the-self/" target="_blank">Metaprogramming in Ruby: It&rsquo;s All About the Self</a></p>
