---
layout: post
status: publish
published: true
title: Git Branching Model
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1445
wordpress_url: http://www.rubycoloredglasses.com/?p=1445
date: '2013-02-06 19:57:45 -0800'
date_gmt: '2013-02-06 19:57:45 -0800'
categories:
- Uncategorized
tags: []
comments: []
---
<p>I just want to put this here for future reference.</p>
<p>There is a version control branching model known as Git-Flow, which is very similar to the model used on the team I work with. See <a href="http://nvie.com/posts/a-successful-git-branching-model/" target="_blank">A Successful Git Branching Model</a>. This seems to work well for teams that make several separate commits to the 'develop' branch, with different versioned releases provided to the 'release branch' that may or may not have been tested and put through a quality assurance process, and finally only major updates (not releases) merged into the 'master' branch and tagged with the appropriate version number.</p>
<p>My team doesn't make small updates here and there on the develop branch. We have our own feature branches, which have all the small micro commits made to them locally with whatever notes we choose to make for each. Once we're done with our work, we squash the commit into a single monolithic commit using 'git rebase' then merge this into the 'develop' branch. This squash makes the process of reviewing the changes with the lead developer easier to do using <a href="http://gitx.frim.nl/" target="_blank">GitX</a>.</p>
<p>To squash we do a rebase with the remote 'develop' branch.</p>
<pre class="brush:shell">git rebase -i origin/develop</pre><br />
After running this rebase command, our configured text editor opens. All of the commits that are not part of the 'develop' branch are displayed with 'pick' shown before them, in order of oldest to newest commit.</p>
<pre class="brush:shell">pick 2df148b combined comments<br />
pick 32e2471 Added description to rake tasks<br />
pick 17ffc55 updated comment for config task<br />
pick f0c4c6a added descriptive comments</p>
<p># Rebase 3012af2..f0c4c6a onto 3012af2<br />
#<br />
# Commands:<br />
# p, pick = use commit<br />
# r, reword = use commit, but edit the commit message<br />
# e, edit = use commit, but stop for amending<br />
# s, squash = use commit, but meld into previous commit<br />
# f, fixup = like "squash", but discard this commit's log message<br />
# x, exec = run command (the rest of the line) using shell<br />
#<br />
# These lines can be re-ordered; they are executed from top to bottom.<br />
#<br />
# If you remove a line here THAT COMMIT WILL BE LOST.<br />
#<br />
# However, if you remove everything, the rebase will be aborted.<br />
#<br />
# Note that empty commits are commented out</pre><br />
To squash everything together we simply replace 'pick' with 's' (or 'squash') for all but the first of the commits. After saving and closing the file, the rebase process continues, various conflicts must be resolved and committed before using 'git rebase --continue'. Once all the commits are squashed together with no further conflicts, the text editor pops open again and we are prompted to create a single commit comment.</p>
<p>After this is complete, we merge our feature branch with it's single commit into the 'develop' branch and push remotely. Next we merge this into the 'release' branch, then into the 'master' branch. This is kind of silly, really for our team with this process we only need a 'master' branch. Our feature branches go through the Q&amp;A and code review process with the lead developer, so really 'master' is all that's necessary.</p>
<p>So in effect, what we do is closer to the <a href="http://scottchacon.com/2011/08/31/github-flow.html" target="_blank">Github-Flow</a>. Our lead developer provided me with this link and we're switching to this model soon, cutting out the 'release' and 'develop' branches. Here is a slideshow by Zach Holman of Github elaborating further - <a href="http://zachholman.com/talk/how-github-uses-github-to-build-github/" target="_blank">How GitHub uses GitHub to Build GitHub</a>.</p>
