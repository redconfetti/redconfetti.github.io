---
layout: post
title: Metaclass
date: '2012-09-19 19:19:03 -0700'
categories:
- Ruby
tags:
- metaprogramming
- metaclass
comments: []
---
I ran into an instance of meta programming in Ruby today, in the
[Exceptional Ruby] book I'm reading for work. It seems that the theme this week
is "you don't know Ruby as well as you could".

[exceptional ruby]: http://exceptionalruby.com/

I might be wrong in my understanding here, but this is what I understand thus
far:

Ruby stores methods for an object in it's class, not the object itself. Objects
only really store their attributes/variables in memory. However there exists some
unseen entity known as the metaclass which belongs to each object, and it can
possibly store methods which belong to that object, but not necessarily to that
objects class.
<!--more-->

```ruby
class Person
  def speak
    puts "Hello There!"
  end
end

john = Person.new
bob = Person.new

class << john
  def bark
    puts "Ruff! Ruff!"
  end
end

>> john.speak
=> Hello There!

>> bob.speak
=> Hello There!

>> john.bark
=> Ruff! Ruff!

>> bob.bark
NoMethodError: undefined method `bark' for #
```

The reference to 'class << john' opens a code block where methods are defined
in the metaclass for 'john', and not the 'Person' class.

A more thorough understanding of this is explored in this blog post -
[Metaprogramming in Ruby: It's All About the Self]

[Metaprogramming in Ruby: It's All About the Self]: http://yehudakatz.com/2009/11/15/metaprogramming-in-ruby-its-all-about-the-self/
