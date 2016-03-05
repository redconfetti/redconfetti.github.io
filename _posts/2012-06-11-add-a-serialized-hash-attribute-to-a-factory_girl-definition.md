---
layout: post
status: publish
published: true
title: Add a Serialized Hash Attribute to a Factory_Girl Definition
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1279
wordpress_url: http://www.rubycoloredglasses.com/?p=1279
date: '2012-06-11 20:25:13 -0700'
date_gmt: '2012-06-11 20:25:13 -0700'
categories:
- Testing
tags:
- factory_girl
- hash
comments:
- id: 373
  author: Josh
  author_email: jclayton@thoughtbot.com
  author_url: ''
  date: '2012-07-05 21:28:48 -0700'
  date_gmt: '2012-07-05 21:28:48 -0700'
  content: 'Alternatively you could do either of these: https://gist.github.com/3056591'
- id: 374
  author: redconfetti
  author_email: jason@redconfetti.com
  author_url: http://www.redconfetti.com/
  date: '2012-07-05 22:15:28 -0700'
  date_gmt: '2012-07-05 22:15:28 -0700'
  content: Oh true. Thanks Josh!
- id: 7349
  author: Claudio
  author_email: claudiosikeda@hotmail.com
  author_url: ''
  date: '2013-04-05 16:51:41 -0700'
  date_gmt: '2013-04-05 16:51:41 -0700'
  content: Thank you!
- id: 15396
  author: Nicolas Sebastian Vidal
  author_email: nicolas.s.vidal@gmail.com
  author_url: ''
  date: '2013-09-04 15:32:12 -0700'
  date_gmt: '2013-09-04 15:32:12 -0700'
  content: thank you! very usefull.
- id: 29934
  author: Dominic
  author_email: dewolfe@easymanifest.com
  author_url: http://easymanifest.com
  date: '2014-04-05 20:15:18 -0700'
  date_gmt: '2014-04-05 20:15:18 -0700'
  content: I've been trying to figure out why my factory wasn't working..Thanks a
    lot for this.
- id: 72780
  author: Carl
  author_email: carlos.eden.c1@gmail.com
  author_url: http://etrade.crimsonlogic.com/
  date: '2015-05-13 09:33:30 -0700'
  date_gmt: '2015-05-13 09:33:30 -0700'
  content: Was able to get this to work. Thanks!
- id: 73298
  author: Fern
  author_email: fernccarlos1@gmail.com
  author_url: http://www.sourcefit.com/web-development
  date: '2015-06-01 01:24:02 -0700'
  date_gmt: '2015-06-01 01:24:02 -0700'
  content: Thanks for this. I used the 2nd option and it worked.:)
---
<p>I recently declared an ActiveRecord model which stores a serialized Hash inside of a text field. When I tried to setup a factory for this model using FactoryGirl, I received many syntax errors. This is because FactoryGirl attributes expect a single value or a certain form of code block.</p>
<pre class="brush:rails">  factory :post do<br />
    title        "Example Post"<br />
    body         "This is the body of the example post"<br />
    meta         { "version" => 2 }<br />
    created_at   "2012-06-01 17:53:13"<br />
  end<br />
</pre><br />
To include a hash as an attribute of a factory, declare the Hash separately and then simply assign it directly in the factory definition.</p>
<pre class="brush:rails">  meta_hash = { :version => 2 }</p>
<p>  factory :post do<br />
    title        "Example Post"<br />
    body         "This is the body of the example post"<br />
    meta         meta_hash<br />
    created_at   "2012-06-01 17:53:13"<br />
  end<br />
</pre><br />
As <a href="https://github.com/joshuaclayton" target="_blank">Joshua Clayton</a> pointed out, one could also do <a href="https://gist.github.com/joshuaclayton/3056591" target="_blank">the following:</a></p>
<pre class="brush:rails">factory :post do<br />
  title        "Example Post"<br />
  body         "This is the body of the example post"</p>
<p>  meta         { { version: 2 } }<br />
  # or<br />
  meta({ version: 2 })</p>
<p>  created_at   "2012-06-01 17:53:13"<br />
end<br />
</pre></p>
