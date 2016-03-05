---
layout: post
status: publish
published: true
title: Bypassing the AngularJS router for anchor tags
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1829
wordpress_url: http://www.rubycoloredglasses.com/?p=1829
date: '2014-09-18 19:19:44 -0700'
date_gmt: '2014-09-18 19:19:44 -0700'
categories:
- AngularJS
tags:
- AngularJS
- ngRoute
- routing
- anchor
- CSV download
---
<p>I'm working with a Rails application that is using an AngularJS front-end. We are using routing to override the behavior of anchor tags to ensure that they load other templates with controllers, as defined in our <a href="https://github.com/ets-berkeley-edu/calcentral/blob/d738be94/app/assets/javascripts/angular/configuration/routeConfiguration.js#L12" target="_blank">routeConfiguration.js</a>. This works out great most of the time, unless you need to override the routing so that your anchor tag can point to an end-point served by your Rails back-end. In my case, I'm linking to an end-point that serves a named CSV file. Without any sort of over-ride, I was finding that the default fallback behavior defined by the <a href="https://docs.angularjs.org/api/ngRoute/provider/$routeProvider" target="_blank">otherwise() method</a> was occurring. In my case this was a 404 page template that loaded.</p>
<p>After much searching, the following solution was found. All you have to do is simply add a target attribute to the anchor tag with the value "_self".</p>
<pre class="brush:html">
<a href="/download.csv" target="_self">Download CSV File</a></p>
<p></pre><br />
Much thanks to <a href="https://coderwall.com/p/em4vua" target="_blank">BJ Basa&ntilde;es</a> for posting this solution. I'm re-posting it here so that it's not lost forever, and also so that I can lend additional keywords to lead others to this solution.</p>
<p>Update: It turns out that this is mentioned in the <a href="https://docs.angularjs.org/guide/$location" target="_blank">documentation for $location</a>.</p>
