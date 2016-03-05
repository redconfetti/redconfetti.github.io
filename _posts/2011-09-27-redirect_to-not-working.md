---
layout: post
title: Redirect_to not working
date: '2011-09-27 16:37:59 -0700'
categories:
- Ruby on Rails
tags:
- http post
comments: []
---
<p>I was just working on a Ruby on Rails controller method that receives information from the previous form via HTTP POST. I coded it so that if certain form variables weren't present it would set a flash message and redirect to the form page. I tried and tried and still the redirect wasn't working. I reset my web server, and even restarted my computer, but stil this didn't resolve the issue.</p>
<p>I then realized that perhaps redirects aren't possible with HTTP POST's, only GET requests. I ended up just creating a generic view for displaying errors, and will render that view and then 'return FALSE' inside of the if statement when an error is detected.</p>
