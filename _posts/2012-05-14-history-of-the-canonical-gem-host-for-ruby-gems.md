---
layout: post
status: publish
published: true
title: History of the Canonical Gem Host for Ruby Gems
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1217
wordpress_url: http://www.rubycoloredglasses.com/?p=1217
date: '2012-05-14 18:41:23 -0700'
date_gmt: '2012-05-14 18:41:23 -0700'
categories:
- Gems
tags: []
comments: []
---
<p>The default repository for downloading gems using RubyGems was originally RubyForge.org. This was likely because the <a href="https://rubyforge.org/projects/rubygems/" target="_blank">RubyGems project</a> was hosted only from RubyForge. This meant that when you ran 'gem install rails', your RubyGems installation was configured to download the gem from 'gems.rubyforge.org'. In August of 2008 Github started <a href="http://www.binarylogic.com/2008/08/31/rubyforge-overshadowed-by-github/" target="_blank">gaining popularity</a> amongst the Ruby community after it started providing it's own gem server via gems.github.com. This resulted in many Ruby developers configuring RubyGems to use the Github server as a <a href="http://www.infoq.com/news/2008/08/gems-from-rubyforge-and-github" target="_blank">secondary source of gems</a>.</p>
<p>In August of 2009 a new gem hosting repository came onto the scene known as <a href="http://www.rubyinside.com/gemcutter-a-fast-and-easy-approach-to-ruby-gem-hosting-2281.html" target="_blank">GemCutter</a>, aiming to resolve issues caused by how Github and RubyForge were handling hosting of gems. In September they decided to move the service to <a href="http://update.gemcutter.org/2009/09/25/kinetic-energy.html" target="_blank">RubyGems.org</a>. In October of Github <a href="https://github.com/blog/515-gem-building-is-defunct" target="_blank">discontinued building and serving gems</a> from gems.github.com, and RubyGems.org became the <a href="http://update.gemcutter.org/2009/10/26/transition.html" target="_blank">official default host for Ruby gems</a>.</p>
