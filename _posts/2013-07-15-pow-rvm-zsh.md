---
layout: post
title: POW RVM ZSH
date: '2013-07-15 00:07:38 -0700'
categories:
- Ruby on Rails
tags:
- rvm
- pow
- zsh
comments: []
---
I'm using a Rails 3 app, and my colleague updated the RVM config to use Ruby 2.0.0. I was having issues getting POW to work with the app. I'm using ZSH as my shell also.

The following command resolved my issue.

``` shell
rvm env . -- --env > .powenv```

Props to <a href="http://stackoverflow.com/questions/10154928/pow-rvm-and-zsh-not-working-together" target="_blank">Linus on StackOverflow</a> for this solution.

 

