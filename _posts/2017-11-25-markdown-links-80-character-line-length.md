---
layout: post
published: true
title: Markdown Links and 80 Character Line Length
description: How to better format anchors/links within Markdown
image_url: /images/posts/2017-11-25-markdown-links.png
date: 2017-11-25 09:55:00 -0500
categories:
- Development
tags:
- markdown
---

I've long been a fan of using [Markdown] for documentation in projects
hosted on Github. In October of 2014 I decided to migrate from a Wordpress
blog to [Github Pages], which is powered by limited [Jekyll]
functionality on the Github server side.

With this migration I converted all my articles from HTML to
[Github Flavored Markdown (GFM)], which resulted in much better support for
formatting my code examples, tables, strikethrough text formatting, and
[emojii][Github Markdown - Emojii].
<!--more-->

I've made use of the following cheat sheets for Markdown syntax:

* [Github Markdown Cheatsheet]
* [Daring Fireball - Markdown: Syntax]
* [Adam-P Markdown Cheatsheet]

An issue I've had is trying to limit the line length of my markdown. Line
breaks do not break apart paragraphs when rendered, however you cannot add line
breaks into the URL of the link without breaking the link.

## Alternative Syntax

Typically link syntax is provided like so:

```markdown
[Google](http://www.google.com/)
```

However if the link and link text exceeds 80 characters, then you end up with
hard to read Markdown.

![Link exceeding limit][Sublime example of link exceeding 80 character limit]

The solution is to use a syntax not commonly mentioned in Markdown guides for
linking that separates the text content from the URL.

The links may still wrap at the bottom of the document, but at least the
content is easy to read in plain text, even on the command line.

``` markdown
My Links:

* [Github Flavored Markdown][1]
* [Basic Writing and Formatting Syntax - Using Emoji][2]

[1]: https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown
[2]: https://help.github.com/articles/basic-writing-and-formatting-syntax/#using-emoji
```

One problem I've run into is that the numbered references end up provided
throughout the page out of order, which bothers me in some obsessive-compulsive
sort of way.

Instead you can use the link text itself.

``` markdown
Check out [Spotify] for cool music

[Spotify]: https://www.spotify.com/
```

Alternatively, you can also use a case insensitive text key if the link text is
too informal for you.

``` markdown
[Click here for Google][Google Link]

[google link]: http://www.google.com/
```

## Images

A similar alternative for images can also be used:

``` markdown
![Beautiful flower photo][flower photo]

[flower photo]: /images/flower.png
```

[Markdown]: https://en.wikipedia.org/wiki/Markdown
[Github Pages]: https://help.github.com/articles/what-is-github-pages/
[Jekyll]: https://jekyllrb.com/
[Github Flavored Markdown (GFM)]: https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown
[Github Markdown - Emojii]: https://help.github.com/articles/basic-writing-and-formatting-syntax/#using-emoji
[Github Markdown Cheatsheet]: https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf
[Daring Fireball - Markdown: Syntax]: https://daringfireball.net/projects/markdown/syntax
[Adam-P Markdown Cheatsheet]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
[Sublime example of link exceeding 80 character limit]: /images/posts/2017-11-25-markdown-links.png
