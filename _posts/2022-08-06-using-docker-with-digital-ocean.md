---
layout: post
published: false
title: Using Docker with Digital Ocean
date: 2022-08-06 16:30:00 -0400
comments: true
categories:
  - docker
tags:
  - digital ocean
---

I purchased the book [Docker for Rails Developers] a while back, and I think
that I might have better luck managing what I'm hosting if I'm using Docker.

I was going to try to setup my own build service to avoid using Netlify or
Render, but honestly I can just use Github Actions.

One of the blockers for me was trying to get a build service setup. I wanted
to setup a lightweight web service that could receive a notification
from a [Github Webhook], and that would trigger the build process. The process
would have to occur within an account of some sort, which 

[Github Webhook]: https://docs.github.com/en/developers/webhooks-and-events/webhooks/about-webhooks
[Docker for Rails Developers]: https://pragprog.com/titles/ridocker/docker-for-rails-developers/
[Configure GitHub Actions with Docker]: https://docs.docker.com/ci-cd/github-actions/
[Build and Deploy your first image to your first cluster]: https://docs.digitalocean.com/tutorials/build-and-deploy-your-first-image-to-your-first-cluster/
