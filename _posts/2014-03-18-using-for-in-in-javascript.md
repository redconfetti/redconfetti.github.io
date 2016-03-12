---
layout: post
title: Using 'for in' in Javascript
date: '2014-03-18 02:04:24 -0700'
categories:
- Javascript
tags:
- javascript
- JsLint
---
Today our lead front-end developer pointed out to me that when using a 'for in' loop in Javascript that you want to make sure to use hasOwnProperty() on the element to make sure it belongs to the object, and not properties that were inherited through the prototype chain.

More information is available on [this page](http://www.jslint.com/lint.html#forin) describing common Javascript code mistakes caught by JSLint.

