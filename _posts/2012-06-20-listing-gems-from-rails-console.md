---
layout: post
title: Listing Gems from Rails Console
date: '2012-06-20 21:59:03 -0700'
---

Got this from [Stack Overflow], figured it could come in handy at some point in
the future.

``` ruby
Gem.loaded_specs.values.map {|x| "#{x.name} #{x.version}"}
```

[Stack Overflow]: http://stackoverflow.com/questions/2747990/is-there-any-way-to-tell-which-gems-and-plugins-are-loaded-at-runtime-for-a-rail
