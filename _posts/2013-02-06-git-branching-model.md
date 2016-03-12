---
layout: post
title: Git Branching Model
date: '2013-02-06 19:57:45 -0800'
categories:
- Uncategorized
tags: []
comments: []
---
I just want to put this here for future reference.

There is a version control branching model known as Git-Flow, which is very similar to the model used on the team I work with. See [A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/). This seems to work well for teams that make several separate commits to the 'develop' branch, with different versioned releases provided to the 'release branch' that may or may not have been tested and put through a quality assurance process, and finally only major updates (not releases) merged into the 'master' branch and tagged with the appropriate version number.

My team doesn't make small updates here and there on the develop branch. We have our own feature branches, which have all the small micro commits made to them locally with whatever notes we choose to make for each. Once we're done with our work, we squash the commit into a single monolithic commit using 'git rebase' then merge this into the 'develop' branch. This squash makes the process of reviewing the changes with the lead developer easier to do using [GitX](http://gitx.frim.nl/).

To squash we do a rebase with the remote 'develop' branch.

``` shell
git rebase -i origin/develop
```

After running this rebase command, our configured text editor opens. All of the commits that are not part of the 'develop' branch are displayed with 'pick' shown before them, in order of oldest to newest commit.

``` shell
pick 2df148b combined comments
pick 32e2471 Added description to rake tasks
pick 17ffc55 updated comment for config task
pick f0c4c6a added descriptive comments
# Rebase 3012af2..f0c4c6a onto 3012af2
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

To squash everything together we simply replace 'pick' with 's' (or 'squash') for all but the first of the commits. After saving and closing the file, the rebase process continues, various conflicts must be resolved and committed before using 'git rebase --continue'. Once all the commits are squashed together with no further conflicts, the text editor pops open again and we are prompted to create a single commit comment.

After this is complete, we merge our feature branch with it's single commit into the 'develop' branch and push remotely. Next we merge this into the 'release' branch, then into the 'master' branch. This is kind of silly, really for our team with this process we only need a 'master' branch. Our feature branches go through the Q&amp;A and code review process with the lead developer, so really 'master' is all that's necessary.

So in effect, what we do is closer to the [Github-Flow](http://scottchacon.com/2011/08/31/github-flow.html). Our lead developer provided me with this link and we're switching to this model soon, cutting out the 'release' and 'develop' branches. Here is a slideshow by Zach Holman of Github elaborating further - [How GitHub uses GitHub to Build GitHub](http://zachholman.com/talk/how-github-uses-github-to-build-github/).

