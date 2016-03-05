---
layout: post
status: publish
published: true
title: Ruby Strftime
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1601
wordpress_url: http://www.rubycoloredglasses.com/?p=1601
date: '2013-07-26 22:16:42 -0700'
date_gmt: '2013-07-26 22:16:42 -0700'
categories:
- Ruby
tags:
- dates
- times
---
<p>Instead of piecing together Ruby strftime strings to use for various formats each time, I'm making this post to store common variations for me to reference later.</p>
<p>I used the legend posted by <a href="http://apidock.com/users/annaswims" target="_blank">annaswims</a> on <a href="http://apidock.com/rails/ActiveSupport/TimeWithZone/strftime" target="_blank">ApiDock.com</a> to piece these together. Thanks Anna.</p>
<pre class="brush:ruby"># Pretty Abbreviated<br />
Time.now.strftime("%a %b %d, %Y %l:%M:%S %p %Z") # => "Fri Jul 26, 2013  3:06:04 PM PDT"</p>
<p># Pretty Long<br />
Time.now.strftime("%A %B %d, %Y %l:%M:%S %p %Z") # => "Friday July 26, 2013  3:06:53 PM PDT"</p>
<p># Short but Human<br />
Time.now.strftime("%-m/%d/%y - %-l:%M:%S %p %Z") # => "7/26/13 - 3:10:15 PM PDT"</p>
<p># Logger Style<br />
Time.now.strftime("%m/%d/%y - %H:%M:%S %Z") # "07/26/13 - 15:13:53 PDT"</p>
<p># ISO8601 format<br />
Time.now.utc.strftime('%FT%H:%MZ') # => "2013-07-26T22:15Z"</p>
<p># DateTime format used with ActiveRecord<br />
Time.now.utc.strftime('%F %H:%M:%S') # => "2013-07-26 22:19:09"</p>
<p></pre><br />
 </p>
