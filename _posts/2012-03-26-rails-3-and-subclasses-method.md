---
layout: post
title: Rails 3 and Subclasses Method
date: '2012-03-26 18:38:14 -0700'
comments: true
categories:
- Model
tags:
- rails3.1
---

I was just trying to create coding that reflectively loads the subclasses of a
class I've defined. The idea is that as new subclasses are added, the script
I'm writing can detect which ones are present and inform a remote API that
support for a specific API feature is available.

I had executed the method once on the class, and it did return the name of the
subclass that I had defined and checked for the existence of in the Rails
console environment. Then I added other subclasses, reloaded (reload!), and
ran the method once again. This time I got nothing but an empty array returned.
It turns out that Rails 3 uses autoloading for classes...so the subclasses
have to be referenced at some point, and thus loaded into memory, before the
subclasses method will include them in the list.
