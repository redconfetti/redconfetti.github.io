---
layout: post
published: true
title: Sidekiq with Cloud66
description: Configuration Tip
image_url: /images/posts/2018-04-24-cloud-66.png
date: 2018-04-24 20:08:00 -0700
categories:
- rails
tags:
- hosting
- cloud66
- sidekiq
---

I had to configure a Rails application using Sidekiq
as the background job processor with Cloud66 recently.

We're currently only using Cloud66 with our staging server.
The following configuration in a `Procfile` in the root
of our repository with the following configuration worked
fine.

```yaml
worker: bundle exec sidekiq -e $RAILS_ENV -C config/sidekiq.yml -i {{UNIQUE_INT}}
```

This was derived from the instructions in the
[How to run Background processes][0] guide.

Cloud66 manages the worker processes cnofigured in this manner using the [BluePill gem][2].

You can log into your server using the [CX toolbelt][1] and use the following commands to inspect
the status of the worker processes.

```shell
# check status of running processes
sudo bluepill status

# view log for worker
sudo bluepill log user_worker_1
```

Worker configuration in the form of .pill files are located on the server within
`/etc/bluepill/autoload`. Worker logs are stored in the logs folder for the application
(e.g. `/var/deploy/heroiq/web_head/current/log/user_worker_1.log`).

[0]: https://help.cloud66.com/rails/how-to-guides/deployment/proc-files.html
[1]: https://help.cloud66.com/maestro/quickstarts/using-cloud66-toolbelt.html
[2]: https://github.com/bluepill-rb/bluepill
