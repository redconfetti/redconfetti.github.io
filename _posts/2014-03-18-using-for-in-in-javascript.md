---
layout: post
status: publish
published: true
title: Using 'for in' in Javascript
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1760
wordpress_url: http://www.rubycoloredglasses.com/?p=1760
date: '2014-03-18 02:04:24 -0700'
date_gmt: '2014-03-18 02:04:24 -0700'
categories:
- Javascript
tags:
- javascript
- JsLint
---
<p>Today our lead front-end developer pointed out to me that when using a 'for in' loop in Javascript that you want to make sure to use hasOwnProperty() on the element to make sure it belongs to the object, and not properties that were inherited through the prototype chain.</p>
<p>More information is available on <a href="http://www.jslint.com/lint.html#forin" target="_blank">this page</a> describing common Javascript code mistakes caught by JSLint.</p>
