---
layout: post
published: false
title: Running a BlueSky Social node with Docker
date: '2023-10-06 12:34:00 -0400'
categories:
  - docker
tags:
  - docker
  - bluesky
---

I recently accepted an invite to join [BlueSky Social][], an alternative to
[X] (formerly known as Twitter) based on an open protocol known as
[Authenticated Transfer (AT) Protocol][]

[BlueSky Social]: https://bsky.app/
[X]: https://twitter.com/home
[Authenticated Transfer (AT) Protocol]: https://atproto.com/

I'm really excited for this because it has the most promise of ending social
media that is centralized, and thus moderated by a single organization. Much like
you can choose your own email provider on the internet, because SMTP and POP3
are internet standards, I want to adopt and support a protocol that supports
an open standard for social media.

## Running a Node

Setting up your own node starts with the
[Federation Developer Sandbox Guidelines][]

A node/server that is part of the BlueSky network is known as a Personal Data
Server (PDS). See [BlueSky-Social PDS] for the server software.

[Federation Developer Sandbox Guidelines]: https://atproto.com/blog/federation-developer-sandbox
[BlueSky-Social PDS]: https://github.com/bluesky-social/pds
