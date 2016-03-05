---
layout: post
title: Custom Rake Tasks Not Loading
date: '2012-01-01 18:57:34 -0800'
categories:
- Ruby on Rails
tags:
- rake
comments:
- id: 335
  author: Anon
  author_email: anon@example.com
  author_url: ''
  date: '2012-01-08 06:09:51 -0800'
  date_gmt: '2012-01-08 10:09:51 -0800'
  content: You may also notice, in Rails 3.1.3 at least, that if you leave of the
    "desc" or the desc is ".blank?" then rake -T will not show that task. You may
    have noticed that a few tasks that used to be visible in rake -T prior to Rails
    3, no longer show up. That's because the rails team decided to trim the list of
    tasks displayed and, for example in databases.rake, you will find that the "desc"
    for those tasks is commented out. Now you know another reason rake -T may not
    show rake tasks. :)
- id: 366
  author: Andrey Viana
  author_email: andreydjason@gmail.com
  author_url: http://andreyviana.com
  date: '2012-06-11 20:43:49 -0700'
  date_gmt: '2012-06-11 20:43:49 -0700'
  content: Thanks man, I didnt realize that too :)
---
I recently went to create a new Rake task under /lib/tasks in a Rails application I'm working on. I didn't understand why the rake tasks weren't showing when I would run 'rake -T' from the command line.

When I'd try to run the task itself I would get a 'Don't know how to build task' error.

I just realized that I was naming my task file with the '.rb' extension, and not '.rake'. Doh!

