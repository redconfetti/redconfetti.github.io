---
layout: post
status: publish
published: true
title: Deleting Git Branches in Remote Repository
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1271
wordpress_url: http://www.rubycoloredglasses.com/?p=1271
date: '2012-01-24 18:08:06 -0800'
date_gmt: '2012-01-24 18:08:06 -0800'
categories:
- Development
tags:
- git
comments: []
---
<p>I had recently used a branch to handle all the modifications I was making to a system for a Rails 3.1 upgrade from Rails 2.4.3. After I merged my changes back into the master branch, I deleted the 'rails3' branch locally, but it still remained on the remote server.</p>
<p>I found that 'git push origin :branch_name' will delete the repository from the remote server if the branch has been removed locally.</p>
<pre class="brush:shell">
$ git push origin :rails3<br />
To git@redconfetti.com:myrepo.git<br />
 - [deleted]         rails3<br />
</pre></p>
