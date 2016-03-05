---
layout: post
status: publish
published: true
title: Vagrant SSH Failure - Connection closed by remote host
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1905
wordpress_url: http://www.rubycoloredglasses.com/?p=1905
date: '2015-09-10 19:21:52 -0700'
date_gmt: '2015-09-10 19:21:52 -0700'
categories:
- Development
tags: []
---
<p>
  I recently was running into issues with Vagrant where I'd start the virtual machine using the 'vagrant up' command, 
  but I'd receive an error when trying to use {% highlight shell %}vagrant ssh{% endhighlight %}.
</p>

{% highlight shell %}
$ vagrant ssh
ssh_exchange_identification: Connection closed by remote host
{% endhighlight %}

<p>
  I'm using a Vagrant/Ansible configuration based on <a href="https://github.com/roots/trellis/" target="_blank">roots/trellis</a>.
</p>
<p>I tried to look into the issue further by running the command with verbose output.</p>
<pre><code>$ vagrant ssh -- -vvv<br />
</code></pre></p>
<p>I noticed that it's trying to use 127.0.0.1 to SSH into the VM on port 2222. When I try to SSH manually using <code>ssh vagrant@192.168.50.5 -p 2222</code> it works fine, but with 127.0.0.1 I get the error still. It seemed that connecting to the VM from 127.0.0.1 is triggering some sort of block.</p>
<p>I tried to check /var/log/syslog and /var/log/auth.log (with SSHD configured for verbose mode). I don't see any log for the failed attempt in the auth.log, though I do see a normal login. It doesn't seem like the connection is being blocked.</p>
<p>I reported this issue to the roots/trellis project - <a href="https://github.com/roots/trellis/issues/348">#348</a></p>
<p>After further investigation I realized that I was configuring sshd on the virtual machine to use port 2222, when really Vagrant or Virtualbox was responsible for forwarding port 2222 on localhost to port 22 of the VM. I was essentially making the port forwarding that it configures invalid by changing SSHD to listen on port 2222.</p>
