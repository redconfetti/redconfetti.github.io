---
layout: post
published: true
title: VueJS - Built-In & Reserved Tags
description: Explains which names are not allowed for use with VueJScomponents
date: 2020-03-28 14:42:00 -0800
categories:
  - vue
tags:
  - vue
---

I'm working on an app that is meant to present a list of "steps" along a certain
"path". I tried to create a component called "path", but this wasn't allowed.

I got this error in the console.

```bash
vue.runtime.esm.js:638 [Vue warn]: Do not use built-in or reserved HTML elements as component id: path
```

It turns out that HTML and SVG tags are reserved and cannot be used as VueJS
component names.

- [Built-In Tags](https://github.com/vuejs/vue/blob/a59e05c2f/dist/vue.js#L134)
  - slot
  - component
- [Reserved Tags](https://github.com/vuejs/vue/blob/a59e05c2f/dist/vue.js#L5614,L5616)
  - [HTML Tags](https://github.com/vuejs/vue/blob/a59e05c2f/dist/vue.js#L5589,L5601)
    - html
    - body
    - base
    - head
    - link
    - meta
    - style
    - title
    - address
    - article
    - aside
    - footer
    - header
    - h1
    - h2
    - h3
    - h4
    - h5
    - h6
    - hgroup
    - nav
    - section
    - div
    - dd
    - dl
    - dt
    - figcaption
    - figure
    - picture
    - hr
    - img
    - li
    - main
    - ol
    - p
    - pre
    - ul
    - a
    - b
    - abbr
    - bdi
    - bdo
    - br
    - cite
    - code
    - data
    - dfn
    - em
    - i
    - kbd
    - mark
    - q
    - rp
    - rt
    - rtc
    - ruby
    - s
    - samp
    - small
    - span
    - strong
    - sub
    - sup
    - time
    - u
    - var
    - wbr
    - area
    - audio
    - map
    - track
    - video
    - embed
    - object
    - param
    - source
    - canvas
    - script
    - noscript
    - del
    - ins
    - caption
    - col
    - colgroup
    - table
    - thead
    - tbody
    - td
    - th
    - tr
    - button
    - datalist
    - fieldset
    - form
    - input
    - label
    - legend
    - meter
    - optgroup
    - option
    - output
    - progress
    - select
    - textarea
    - details
    - dialog
    - menu
    - menuitem
    - summary
    - content
    - element
    - shadow
    - template
    - blockquote
    - iframe
    - tfoot
  - [SVG Tags](https://github.com/vuejs/vue/blob/a59e05c2f/dist/vue.js#L5603,L5610)
    - svg
    - animate
    - circle
    - clippath
    - cursor
    - defs
    - desc
    - ellipse
    - filter
    - font-face
    - foreignObject
    - g
    - glyph
    - image
    - line
    - marker
    - mask
    - missing-glyph
    - path
    - pattern
    - polygon
    - polyline
    - rect
    - switch
    - symbol
    - text
    - textpath
    - tspan
    - use
    - view

This includes SVG elements (such as `path`) as well as HTML elements.
See [isSVG](https://github.com/vuejs/vue/blob/dev/dist/vue.js#L5603,L5610) and
[isHTMLTag](https://github.com/vuejs/vue/blob/a59e05c2/dist/vue.js#L5589,L5601)
