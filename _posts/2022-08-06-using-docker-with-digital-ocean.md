---
layout: post
published: false
title: JamStack CI/CD with Docker and Digital Ocean
date: 2023-03-04 09:30:00 -0500
comments: true
categories:
  - docker
tags:
  - eleventy
  - docker
  - digital ocean
  - jamstack
  - gitlab
---

I recently became more aware of the pros of containerized applications,
and how Github or Gitlab CI/CD is meant to be used with such environments.

For my own personal blog, this tech website, or any other personal pet projects
I want to work on, I don't want to have to pay more than $10 a month for
hosting services. I'd prefer to have a humble little Docker server running
all my little containers, with a self-spun automated build and deploy pipeline.

[Vercel], [Render], and [Netlify] are all nice services, but honestly I don't
trust their free entry plans. I wanted to setup a little site for my own
personal notes with password protection, but that's not possible without paying
the $19-$20 a month fee required by Netlify or Vercel for such a feature. I
know damn well that Apache supports BasicAuth for websites that will work
just fine to host my own public intranet site.

So here's how you can setup your own personal web hosting for $4 - $12 a month.

<!--more-->

1. [Eleventy](https://www.11ty.dev/)
1. Docker Desktop
1. [Gitlab Free Tier account]
1. VPS node with [Digital Ocean] or [Linode] (now Akamai)

* [Build and Deploy your first image to your first cluster]

[Diagram](https://docs.google.com/drawings/d/1ZnFu51UhOB4DkxbAVRmSZnSycMzWQMWYbFS4Kzj4jMo/edit)

## Install Doctl

* [How to Install Doctl](https://docs.digitalocean.com/reference/doctl/how-to/install/)
* [Create a Personal Access Token](https://docs.digitalocean.com/reference/api/create-personal-access-token/)
* [How to use Doctl](https://www.digitalocean.com/community/tutorials/how-to-use-doctl-the-official-digitalocean-command-line-client)
* [Doctl Reference](https://docs.digitalocean.com/reference/doctl/reference/)

You can configure the default context in doctl with your token like so:

```bash
$ doctl auth init -t dop_v1_1234abcdef1234abcdef1234abcdef1234abcdef1234
Using token for context default

Validating token... ✔
```

Configuration for doctl defaults to `.config/doctl/config.yaml`.

## Authenticate Docker with your Registry

[Container Registry Quickstart](https://docs.digitalocean.com/products/container-registry/quickstart/)

```bash
$ doctl registry login
Logging Docker in to registry.digitalocean.com

$ docker login registry.digitalocean.com
Authenticating with existing credentials...
Login Succeeded
```

## Build Image

Build an image from the Dockerfile.prod, and tag for the Digital Ocean registry.

```bash
docker build -f Dockerfile.prod -t registry.digitalocean.com/redconfetti/rails_app:prod .
```

## Push Image to Registry

```bash
docker push registry.digitalocean.com/redconfetti/rails_app:prod
```

[Vercel]: https://vercel.com/
[Render]: https://render.com/
[Netlify]: https://www.netlify.com/
[Gitlab Free Tier account]: https://about.gitlab.com/pricing/
[Digital Ocean]: https://www.digitalocean.com/pricing/droplets#basic-droplets
[Linode]: https://www.linode.com/products/shared/
[Build and Deploy your first image to your first cluster]: https://docs.digitalocean.com/tutorials/build-and-deploy-your-first-image-to-your-first-cluster/
