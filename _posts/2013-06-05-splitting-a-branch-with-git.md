---
layout: post
title: Splitting a Branch with Git
date: '2013-06-05 19:18:38 -0700'
categories:
- Uncategorized
tags:
- git
comments: []
---

There are times that a task you are working on results in an extremely huge
amount of changes. Although you may have been careful, and tested each
modification out well, there is always a possibility that something will cause
an issue in production. If your branch contains modifications that can be
released in separately, without interdependencies, it's a good idea to split
the feature branch into separate releases.

First you'll want to interactively rebase your branch, squash all commits into
a single commit, and then amend the remaining commit so that it's the most
recent.

``` shell
git checkout my_feature_branch

git fetch

git rebase -i origin/master

git commit --amend --reset-author
```
<!--more-->

You can confirm that your last commit which contains all the changes you've
provided in your feature branch is the last one using 'git log'.

Next, create a new branch from your rebased feature branch using a name that
describes the first portion of modifications you're wanting to split off from
your finished feature branch.

``` shell
git checkout -b new_comments_and_docs
```

Then reset your branch to the commit that comes before your squashed commit.
This is practically the state of the last commit in the master branch that you
rebased from.

``` shell
git reset HEAD^
```

If you run 'git status' now, you'll see the list of unstaged/modified files, and
untracked/new files that contain your work from this branch. It's a good idea to
take this list of files and separate them into groups for the split branches you
plan on creating, using 'git diff' on the modified files to review the changes
you made. This will help you avoid mistakenly forgetting to include certain
files during the process.

Once you're ready, simply use 'git add' on the files that contain the changes
you wish to keep in the current split from your feature branch.

After you've added the files/modifications you wish to keep in this branch, and
committed them, run the following commands to clear remaining modifications and
untracked files.

``` shell
git checkout *

git clean -f
```

You now have a split version of your feature branch. Checkout your feature
branch and perform the steps above for the other changes you wish to split into
separate branches.
