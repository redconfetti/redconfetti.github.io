---
layout: post
status: publish
published: true
title: Adding a New User in Ubuntu
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1274
wordpress_url: http://www.rubycoloredglasses.com/?p=1274
date: '2011-12-29 18:08:48 -0800'
date_gmt: '2011-12-29 18:08:48 -0800'
categories:
- Hosting
tags: []
comments: []
---
<h2>Adding User</h2><br />
When setting up a new website manually on an Ubuntu server you need to establish a user account with a home directory, and Bash shell access to the server.</p>
<pre class="brush:shell">useradd -m testuser -s /bin/bash</pre><br />
After creating the account you'll want to assign a password for the account.</p>
<pre class="brush:shell">passwd testuser</pre></p>
