---
layout: post
status: publish
published: true
title: Rails 3 and Subclasses Method
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1105
wordpress_url: http://www.redconfetti.com/?p=1105
date: '2012-03-26 18:38:14 -0700'
date_gmt: '2012-03-26 22:38:14 -0700'
categories:
- Model
tags:
- rails3.1
comments: []
---
<p>I was just trying to create coding that reflectively loads the subclasses of a class I've defined. The idea is that as new subclasses are added, the script I'm writing can detect which ones are present and inform a remote API that support for a specific API feature is available.</p>
<p>I had executed the method once on the class, and it did return the name of the subclass that I had defined and checked for the existence of in the Rails console environment. Then I added other subclasses, reloaded (reload!), and ran the method once again. This time I got nothing but an empty array returned. It turns out that Rails 3 uses autoloading for classes...so the subclasses have to be referenced at some point, and thus loaded into memory, before the subclasses method will include them in the list.</p>
<p> </p>
