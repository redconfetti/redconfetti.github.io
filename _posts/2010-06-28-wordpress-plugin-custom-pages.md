---
layout: post
status: publish
published: true
title: Wordpress Plugin - Custom Pages?
date: '2010-06-28 22:39:15 -0700'
comments: true
categories:
- web development
tags:
- wordpress
- plugin
- permalinks
---

## My Dilema

Okay. I've worked on making a Wordpress plugin once. It's pretty easy to make
a plugin which replaces a tag such as `[another-plugin-tag parameter="value"]`
with some sort of other HTML code. For instance it's pretty straight forward
to replace `[iframe http://www.google.com/ 800 600]` with an iframe tag.

Something I've found difficult to find however is how you can create custom
pages as soon as the plugin is activated, which are accessible using a
permalink such as `http://www.wordpress-site.com/myplugin/search/` which can
submit a form to another URL such as
`http://www.wordpress-site.com/myplugin/results/` and then provide the results
with a URL such as `http://www.wordpress-site.com/myplugin/results/id/3/` or
anything else pretty like that.

<!--more-->

And I'm not talking about searching for posts or pages or anything. I'm
talking about extending Wordpress to have functionality which is not blog
related, while still being a plugin.

I installed the 'Contact Form 7' plugin to see how it submitted the form, and
then I realized it uses Ajax. Great. I don't want Ajax.

## A Hint of a Solution

I searched online looking for something to explain this, because certainly
someone else must have been scratching their head like I have. No guides
seemed to explain this to me. I'd search for 'Wordpress plugin permalinks' and
I'd only find plugins that deal with permalinks somehow (not what I was
looking for).

And I was ignoring all the documentation on hooks and filters, because I don't
want to filter normal blog content, or hook to some blog content. But I was
mistaken. I do want to hook a function to something. It turns out that
Wordpress has a number of actions which it goes through when loading a normal
page, available by name in the [Plugin API Action Reference page][1].

At some point of the page loading the permalink style URL, which is basically
made possible by a mod_rewrite rule which says that any address is processed
by index.php. The Wordpress system determines if the URL relates to a page or
post or something, or otherwise provides a 404 style error. Okay, so if I can
somehow tell Wordpress - "Yes! There is a /myplugin/ page", or "Yes! There is
a /myplugin/results/" page, then I'll have one step of my solution finished.

After further researching I found that there is an article on how
[Wordpress processes a request][2], and it even mentions GET and POST
submissions. This was also obviously hard because 'post' is the term used to
refer to the blog post records, so a search on Google for 'Wordpress post
request' didn't return something relevant.

## To Be Continued

I'm going to continue to investigate how to build the type of plugin which
provides custom URL's, without requiring the existence of pages for these
URLs, and also somehow block the creation of pages which use the permalink
structure used by the plugin.

__Update:__ It's been a while since I posted this, but I found that there are
rewrite rules stored somewhere in a serialized format or something in the
wp_options table.

There are some functions you can use to add or modify the rewrites/routes so
that they point to your custom script. Once a certain rule/route is pointing
to your own plugin script, you can do whatever you want with the requests....
serve up multiple pages, etc.

[1]: http://codex.wordpress.org/Plugin_API/Action_Reference#Actions_Run_During_a_Typical_Request
[2]: http://codex.wordpress.org/Query_Overview
