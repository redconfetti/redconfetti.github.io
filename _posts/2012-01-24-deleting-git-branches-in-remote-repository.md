---
layout: post
title: Deleting Git Branches in Remote Repository
date: '2012-01-24 18:08:06 -0800'
categories:
- Development
tags:
- git
comments: []
---
I had recently used a branch to handle all the modifications I was making to a system for a Rails 3.1 upgrade from Rails 2.4.3. After I merged my changes back into the master branch, I deleted the 'rails3' branch locally, but it still remained on the remote server.

I found that 'git push origin :branch_name' will delete the repository from the remote server if the branch has been removed locally.

``` shell
$ git push origin :rails3
To git@example.com:myrepo.git
 - [deleted]         rails3
```

