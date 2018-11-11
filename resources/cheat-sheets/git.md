---
layout: page
title: Git
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

[Other Git Tips](http://mislav.uniqpath.com/2010/07/git-tips/)

# Table of Contents

* [Man Pages](#man-pages)
* [Git Branching](#git-branching)
* [Git Checkout](#git-checkout)
* [Git Commit](#git-commit)
* [Git Diff](#git-diff)
* [Git Log](#git-log)
* [Git Push](#git-push)
* [Git Rebase](#git-rebase)
* [Git Remote](#git-remote)
* [Git Remove](#git-remove)
* [Git Reset](#git-reset)
* [Git Stash](#git-stash)
* [Tagging](#tagging)
* [Patching](#patching)
* [Misc](#misc)

# Man Pages
``` shell
# Manuals
# You can view the manual pages on any of the commands below by using
# 'man git-' followed by the verbs supported by Git such as 'log' or 'commit'
man git-log
man git-commit
```

# Git Branching
``` shell
# view remote branches
git branch -r

# view local and remote branches
git branch -a

# delete local branch
git branch -d my_branch_name

# rename branch
git branch -m old_name new_name

# delete a tracking branch
git branch -r -d otherguys/master

# rename local and remote branch
git branch -m old_name new_name
git push origin :old_name # delete old remote branch
git push origin new_name # create new branch on remote
git branch --set-upstream-to=origin/new_name
```

# Git Checkout
``` shell
# alternative way to clear all changes
git checkout .

# clear all changes to one file
git checkout path/to/file

# create and switch to new branch
git checkout -b my_branch_name

# create and switch to new branch based on remote branch
git checkout -b local_branch_name origin/remote_branch_name

# create a branch from an earlier commit (time travel is possible!)
git checkout -b oldstuff commit_hash

# create a branch that tracks a remote branch
git checkout -b branch_name remote_name/branch_name

# merge selected files from another branch
git checkout frombranch thedir/thefile.txt anotherdir/anotherfile.txt

# create local branch from remote branch
git checkout -b local_branch_name johndoe/remote_branch_name
```

# Git Commit
``` shell
# update the last commit message
git commit --amend -m "New message"

# update the last commit with current date/time
git commit --amend --reset-author
```

See [Auto-squashing Git Commits][1] | [StackOverflow][2]
```
# append staged changes into previous commit
git add .
git commit --fixup=4321dcba
git rebase --interactive --autosquash 4321dcba^
```

# Git Diff
``` shell
# show unstaged changes since last commit
git diff

# show staged and unstaged changes since last commit
git diff HEAD

# show changes since second to last commit
git diff HEAD^

# show changes since third to last commit
git diff HEAD^^

# show changes since 5 commits ago
git diff HEAD~5

# show changes between most recent and second most recent commit
git diff HEAD^..HEAD

# show changes between two commits
git diff 4fb063f..f5a6ff9

# show changes between tagged release and master
git diff v1.0..master

# show changes between two branches
git diff master my-feature-branch

# show changes using time ranges
git diff --since=1.week.ago --until=1.minute.ago
```

# Git Log
``` shell
# search for all commits (any branch) by message
git log --all --grep="contents of message"

# view commits in oneline format
git log --pretty=oneline

# view commits in oneline format, with decorations
git log --oneline --decorate

# view last 20 logs in reverse with raw comment body only
# good for reporting work performed
git log --reverse --pretty=format:"%B" -20

# view all changes in specific file, ordered from most recent to oldest
git log -p path/to/file

# view last 2 changes in specific file
git log -p -2 path/to/file

# show statistics on changes to files in each commit
git log --stat

# show visual representation of branch merges
git log --graph

# show log in custom format
# %ad - author date
# %an - author name
# %h - SHA hash
# %s subject
# %d ref names
git log --pretty=format:"%h %ad- %s [%an]"

# show with date ranges
git log --until=1.minute.ago
git log --since=1.day.ago
git log --since=1.hour.ago
git log --since=1.month.ago --until=2.weeks.ago
git log --since=2000-01-01 --until=2012-12-21

# setup and use alias for complex git commands
git config --global alias.mylog "log --pretty=format:'%h %s [%an]' --graph"
git mylog
```

# Git Merge-Base
``` shell
# Find the point at which a branch forked from another branch
git merge-base --fork-point master feature_branch
```

# Git Push
``` shell
# push to remote with upstream tracking specified
git push -u origin qa

# push branch to remote repository (origin)
git push origin my_branch_name

# delete remote branch
git push origin :remote_branch_name
```

# Git Rebase
``` shell
# interactive rebase from remote master
git rebase -i origin/master

# interactive rebase from last 4 commits
git rebase -i HEAD~4
```

# Git Remote
``` shell
# view configured remote repositories
git remote -v

# add remote repository
git remote add johndoe https://github.com/johndoe/myproject.git

# remove remote repository
git remote rm johndoe
```

# Git Remove
``` shell
# remove file from repository, without actually deleting
# good for files you only want locally, and have added to .gitignore
git rm --cached mylogfile.log
```

# Git Reset
``` shell
# clear all changes
git reset --hard

# undo a successful merge or commit
git reset --hard HEAD^

# undo a successful commit, keep changes
git reset --soft HEAD^
```

# Git Stash
``` shell
# save current unstaged changes to stash
git stash

# save current unstaged changes to stash with description
git stash save <message>

# view list of stashes
git stash list

# apply first stash to current branch
git stash apply stash@{0}

# drop first stash
git stash drop stash@{0}

# clear all stored stashes
git stash clear
```

# Patching
``` shell
# create patch based on single commit
git format-patch -1 73699d42 --stdout > mycommit.patch

# create patch file (auto generated name) for current feature branch,
# using remote master as base
git format-patch origin/master

# check for errors before apply patch
git apply --check file.patch

# inspect / view statistics for patch
git apply --stat file.patch

# apply a patch
git am file.patch
```

# Tagging
``` shell
# Tag with annotation
git tag -a v1.1 -m "version 1.1 (CodeName: Jason)"

# Tag without annotation
git tag -a v1.2

# Tag a specific commit
git tag -a v1.0 74a360f

# delete git tag locally
git tag -d tagName

# delete remote tag
git push origin :refs/tags/tagName
git push --delete origin tagName

# modify git tag locally
git tag -a v1.23 04567899ae -f

# force push all local tags
git push origin --tags -f

# update local tags from remote
git fetch origin --tags
```

# Misc
```shell
# show log of commits affecting specific file
git whatchanged path/to/file

# display revisions and author for each line of a file (lines 450 - 470)
git blame -L 450,470 lib/file_name.rb

# show commit SHA, author, and date for changes to file
git blame index.html --date short

# apply changes from local commit to current branch
git cherry-pick 04567899ae36651daf3dfa117a1088d594632370

# create a commit that reverts a previous commit
git revert 04567899ae36651daf3dfa117a1088d594632370

# view changes in commit, using SHA hash
git show 6d3b08115028d013d676bc03ece72db3e6e06225

# List tags sorted in descending order, include first 5 in output
git tag -l -n1 --sort=-v:refname | head -n 5
```

[1]: https://robots.thoughtbot.com/autosquashing-git-commits
[2]: https://stackoverflow.com/a/27721031/556737
