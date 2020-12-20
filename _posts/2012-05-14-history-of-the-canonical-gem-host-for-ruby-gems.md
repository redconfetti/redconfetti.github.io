---
layout: post
title: History of the Canonical Gem Host for Ruby Gems
date: '2012-05-14 18:41:23 -0700'
comments: true
categories:
- Gems
tags: []
comments: []
---

The default repository for downloading gems using RubyGems was originally
RubyForge.org. This was likely because the [RubyGems project][1] was hosted only
from RubyForge. This meant that when you ran 'gem install rails', your RubyGems
installation was configured to download the gem from 'gems.rubyforge.org'. In
August of 2008 Github started [gaining popularity][2] amongst the Ruby community
after it started providing it's own gem server via gems.github.com. This
resulted in many Ruby developers configuring RubyGems to use the Github server
as a [secondary source of gems][3].

In August of 2009 a new gem hosting repository came onto the scene known as
[GemCutter][4], aiming to resolve issues caused by how Github and RubyForge were
handling hosting of gems. In September they decided to move the service to
[RubyGems.org][5]. In October of Github
[discontinued building and serving gems][6] from gems.github.com, and
RubyGems.org became the [official default host for Ruby gems][7].

[1]: https://rubyforge.org/projects/rubygems/
[2]: http://www.binarylogic.com/2008/08/31/rubyforge-overshadowed-by-github/
[3]: http://www.infoq.com/news/2008/08/gems-from-rubyforge-and-github
[4]: http://www.rubyinside.com/gemcutter-a-fast-and-easy-approach-to-ruby-gem-hosting-2281.html
[5]: http://update.gemcutter.org/2009/09/25/kinetic-energy.html
[6]: https://github.com/blog/515-gem-building-is-defunct
[7]: http://update.gemcutter.org/2009/10/26/transition.html
