---
layout: post
published: true
future: true
title: RubyGems SSL Error with jRuby
date: '2023-03-22 17:08:51 -0500'
comments: true
categories:
  - Ruby
tags:
  - jruby
  - rubygems
  - bundler
  - ssl
---

I spent several days investigating an error that was coming up with our Rails
application build using jRuby v9.3.3 - v9.3.10. Everytime the build would
try to run `bundle install` we would get the following error.

```bash
Fetching source index from https://rubygems.org/
Retrying fetcher due to error (2/4): Bundler::HTTPError Could not fetch specs from https://rubygems.org/ due to underlying error <Unrecognized SSL message, plaintext connection? (https://rubygems.org/specs.4.8.gz)>

Retrying fetcher due to error (3/4): Bundler::HTTPError Could not fetch specs from https://rubygems.org/ due to underlying error <Unrecognized SSL message, plaintext connection? (https://rubygems.org/specs.4.8.gz)>

Retrying fetcher due to error (4/4): Bundler::HTTPError Could not fetch specs from https://rubygems.org/ due to underlying error <Unrecognized SSL message, plaintext connection? (https://rubygems.org/specs.4.8.gz)>

Could not fetch specs from https://rubygems.org/ due to underlying error
<Unrecognized SSL message, plaintext connection?
(https://rubygems.org/specs.4.8.gz)>
ERROR: bundle install failed
```

You will notice it attempts 3 times, this is because the script is actually
running:

```shell
bundle install --local --retry 3 || { echo "WARNING: bundle install --local failed, running bundle install"; bundle install --retry 3 || { echo "ERROR: bundle install failed"; exit 1; } }
```

<!--more-->

When we would run our script from the command line, it would work just fine.
When we would try to run it from our Bamboo application (used for Continuous
Integration), it would fail with the above errors.

We're using RVM to manage the different Ruby versions needed by our applications
on the Bamboo server. We thought it might be an issue with Bundler, but it
turned out that one of our JAVA configurations was causing this issue.

We noticed that `JAVA_OPTS` was not set when our admin ran the build scripts
from the command line, but with the Bamboo job it was set, and it included
the `-Djava.security.properties` option pointing to a file that used the
following:

```bash
jdk.tls.disabledAlgorithms=SSLv3, RC4, DES, MD5withRSA, DH keySize < 1024, \
    EC keySize < 224, 3DES_EDE_CBC, anon, NULL, RSASSA-PSS
```

When we added this to the export for JAVA_OPTS, the issue occurred when running
the build scripts, or when running the RubyGems troubleshooting script
(`curl -sL https://git.io/vQhWq | ruby`). When we removed it, the error
returned.
