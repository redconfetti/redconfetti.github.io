---
layout: post
published: true
title: Rounded Corners
date: '2009-04-24 17:11:18 -0700'
date_gmt: '2009-04-24 21:11:18 -0700'
categories:
- web development
---

One of the most popular Web 2.0 practices is rounded corners. How do you get them without uploading images, and nesting DIV's,
and worrying about other complications that can break your precious rounded corners?

Answer! jQuery Corners

jQuery Corners is compatible with Firefox, Internet Explorer 6+, Safari (including iPhone), Google Chrome, and Opera 9.0.
All it takes is a simple jQuery style selector call such as the following:

```
<script>
  $(document).ready( function() {
    $('.rounded').corners();
  });
</script>

<div style="background-color:#acc; padding:5px" class="rounded">
Simple Example
</div>
```

You can also experiment further with documented options to change the radius (amount of curve) for the rounded corners, and will
even show properly if there is a background image specified inside of the object with rounded corners.

Available for download at [http://plugins.jquery.com/project/corners](http://plugins.jquery.com/project/corners)

Documentation provided at [http://www.atblabs.com/jquery.corners.html](http://www.atblabs.com/jquery.corners.html)
