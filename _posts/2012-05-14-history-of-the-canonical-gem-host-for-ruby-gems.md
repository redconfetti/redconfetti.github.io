---
layout: post
title: History of the Canonical Gem Host for Ruby Gems
date: '2012-05-14 18:41:23 -0700'
categories:
- Gems
tags: []
comments: []
---
The default repository for downloading gems using RubyGems was originally RubyForge.org. This was likely because the [RubyGems project](https://rubyforge.org/projects/rubygems/) was hosted only from RubyForge. This meant that when you ran 'gem install rails', your RubyGems installation was configured to download the gem from 'gems.rubyforge.org'. In August of 2008 Github started [gaining popularity](http://www.binarylogic.com/2008/08/31/rubyforge-overshadowed-by-github/) amongst the Ruby community after it started providing it's own gem server via gems.github.com. This resulted in many Ruby developers configuring RubyGems to use the Github server as a [secondary source of gems](http://www.infoq.com/news/2008/08/gems-from-rubyforge-and-github).

In August of 2009 a new gem hosting repository came onto the scene known as [GemCutter](http://www.rubyinside.com/gemcutter-a-fast-and-easy-approach-to-ruby-gem-hosting-2281.html), aiming to resolve issues caused by how Github and RubyForge were handling hosting of gems. In September they decided to move the service to [RubyGems.org](http://update.gemcutter.org/2009/09/25/kinetic-energy.html). In October of Github [discontinued building and serving gems](https://github.com/blog/515-gem-building-is-defunct) from gems.github.com, and RubyGems.org became the [official default host for Ruby gems](http://update.gemcutter.org/2009/10/26/transition.html).

