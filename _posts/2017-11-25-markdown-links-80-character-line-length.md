---
layout: post
published: true
title: Markdown Links and 80 Character Line Length
date: 2017-11-25 09:55:00 -0500
categories:
- Development
tags:
- markdown
---

I've long been a fan of using [Markdown][1] for documentation in projects
hosted on Github. In October of 2014 I decided to migrate from a Wordpress
blog to [Github Pages][2], which is powered by limited [Jekyll][3]
functionality on the Github server side.

With this migration I converted all my articles from HTML to
[Github flavored Markdown (GFM)][4], which resulted in much better support for
formatting my code examples, tables, strikethrough text formatting, and
[emojii][5].

I've made use of the following cheat sheets for Markdown syntax:
* [Github Markdown Cheatsheet][6]
* [Daring Fireball - Markdown: Syntax][7]
* [Adam-P Markdown Cheatsheet][8]

An issue I've had is trying to limit the line length of my markdown. Line
breaks do not break apart paragraphs when rendered, however you cannot add line
breaks into the URL of the link without breaking the link.

## Alternative Syntax

Typically link syntax is provided like so:
```
[Google](http://www.google.com/)
```

However if the link and link text exceeds 80 characters, then you end up with
hard to read Markdown.

![Link exceeding limit][9]

The solution is to use a syntax not commonly mentioned in Markdown guides for
linking that separates the text content from the URL.

The links may still wrap at the bottom of the document, but at least the
content is easy to read in plain text, even on the command line.

```
My Links:

* [Github Flavored Markdown][1]
* [Basic Writing and Formatting Syntax - Using Emoji][2]

[1]: https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown
[2]: https://help.github.com/articles/basic-writing-and-formatting-syntax/#using-emoji
```

## Images

A similar alternative for images can also be used:

```
![Beautiful flower photo][1]

[1]: /images/flower.png
```

[1]: https://en.wikipedia.org/wiki/Markdown
[2]: https://help.github.com/articles/what-is-github-pages/
[3]: https://jekyllrb.com/
[4]: https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown
[5]: https://help.github.com/articles/basic-writing-and-formatting-syntax/#using-emoji
[6]: https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf
[7]: https://daringfireball.net/projects/markdown/syntax
[8]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
[9]: /images/posts/2017-11-25-markdown-links.png
