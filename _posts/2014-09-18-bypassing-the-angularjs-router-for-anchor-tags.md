---
layout: post
title: Bypassing the AngularJS router for anchor tags
date: '2014-09-18 19:19:44 -0700'
comments: true
categories:
- AngularJS
tags:
- AngularJS
- ngRoute
- routing
- anchor
- CSV download
---

I'm working with a Rails application that is using an AngularJS front-end. We
are using routing to override the behavior of anchor tags to ensure that they
load other templates with controllers, as defined in our [routeConfiguration.js].
This works out great most of the time, unless you need to override the routing
so that your anchor tag can point to an end-point served by your Rails back-end.
In my case, I'm linking to an end-point that serves a named CSV file. Without
any sort of over-ride, I was finding that the default fallback behavior defined
by the [otherwise() method] was occurring. In my case this was a 404 page
template that loaded.
<!--more-->

After much searching, the following solution was found. All you have to do is
simply add a target attribute to the anchor tag with the value "_self".

``` html
<a href="/download.csv" target="_self">Download CSV File</a>
```

Much thanks to [BJ Basañes](https://coderwall.com/p/em4vua){:target="_blank"}
 for posting this solution. I'm re-posting it here so that it's not lost forever,
 and also so that I can lend additional keywords to lead others to this solution.

Update: It turns out that this is mentioned in the
[documentation for $location](https://docs.angularjs.org/guide/$location).

[routeConfiguration.js]: https://github.com/ets-berkeley-edu/calcentral/blob/d738be94/app/assets/javascripts/angular/configuration/routeConfiguration.js#L12
[otherwise() method]: https://docs.angularjs.org/api/ngRoute/provider/$routeProvider#otherwise
