---
layout: post
title: Open Source Ideas
date: '2013-08-25 22:47:39 -0700'
comments: true
categories:
- Uncategorized
tags: []
---

I've become aware of how important it is to provide a portfolio of things to
share with prospective employers. Having a [Github] profile that shows off all
the Gists and public repositories you've contributed to serves as a perfect
portfolio piece.

Most of the work I've done has been on private systems however, and I've
discarded the code, because I don't own it and don't want legal trouble. So what
can I do that is practical, helpful, and/or fun!?

Here is my list of pet project ideas that I plan on doing in the future.

[Github]: https://www.github.com/

<!--more-->

* Pop Culture Faker - There is a tool that outputs fake names, business names,
  phone numbers, addresses, etc. called [Faker]. Often I find myself using names
  like '[Bob Barker]', or '[Joffrey Baratheon]' in my tests. It would be cool to
  make an alternative version of Faker (or [FFaker]) that pulls from common
  names of celebrities, fictional characters, etc.
* Gemfile Annotator - I remember hearing about a gem that would
  [annotate your Rails models]. I want to make one that adds the name,
  description, and Github project URL for each gem inside of your Gemfile,
  like so:

  ``` ruby
  # Figaro
  # Simple Rails app configuration
  # https://github.com/laserlemon/figaro
  gem "figaro"
  ```

[annotate your Rails models]: https://github.com/ctran/annotate_models
[FFaker]: https://github.com/EmmanuelOga/ffaker
[Joffrey Baratheon]: http://gameofthrones.wikia.com/wiki/Joffrey_Baratheon
[Bob Barker]: http://en.wikipedia.org/wiki/Bob_Barker
[Faker]: http://rubygems.org/gems/faker
