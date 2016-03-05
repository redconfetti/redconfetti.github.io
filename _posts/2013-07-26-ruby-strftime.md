---
layout: post
title: Ruby Strftime
date: '2013-07-26 22:16:42 -0700'
categories:
- Ruby
tags:
- dates
- times
---
Instead of piecing together Ruby strftime strings to use for various formats each time, I'm making this post to store common variations for me to reference later.

I used the legend posted by <a href="http://apidock.com/users/annaswims" target="_blank">annaswims</a> on <a href="http://apidock.com/rails/ActiveSupport/TimeWithZone/strftime" target="_blank">ApiDock.com</a> to piece these together. Thanks Anna.

<pre class="brush:ruby"># Pretty Abbreviated

Time.now.strftime("%a %b %d, %Y %l:%M:%S %p %Z") # => "Fri Jul 26, 2013  3:06:04 PM PDT"

# Pretty Long

Time.now.strftime("%A %B %d, %Y %l:%M:%S %p %Z") # => "Friday July 26, 2013  3:06:53 PM PDT"

# Short but Human

Time.now.strftime("%-m/%d/%y - %-l:%M:%S %p %Z") # => "7/26/13 - 3:10:15 PM PDT"

# Logger Style

Time.now.strftime("%m/%d/%y - %H:%M:%S %Z") # "07/26/13 - 15:13:53 PDT"

# ISO8601 format

Time.now.utc.strftime('%FT%H:%MZ') # => "2013-07-26T22:15Z"

# DateTime format used with ActiveRecord

Time.now.utc.strftime('%F %H:%M:%S') # => "2013-07-26 22:19:09"

```

 

