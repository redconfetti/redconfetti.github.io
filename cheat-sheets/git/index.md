---
layout: page
title: Git
---
[Back to Cheat Sheets](/cheat-sheets/)

[Other Git Tips](http://mislav.uniqpath.com/2010/07/git-tips/)

``` shell
# clear all changes
git reset --hard

# alternative way to clear all changes
git checkout .

# clear all changes to one file
git checkout path/to/file

# create and switch to new branch
git checkout -b my_branch_name

# delete local branch
git branch -d my_branch_name

# rename branch
git branch -m old_name new_name

# push branch to remote repository (origin)
git push origin my_branch_name

# rename local and remote branch
git branch -m old_name new_name
git push origin :old_name # delete old remote branch
git push origin new_name # create new branch on remote
git branch --set-upstream-to=origin/new_name

# remove file from repository, without actually deleting
# good for files you only want locally, and have added to .gitignore
git rm --cached mylogfile.log

# view remote branches
git branch -r

# view local and remote branches
git branch -a

# pull remote branch
git checkout -b local_branch_name origin/remote_branch_name

# delete remote branch
git push origin :remote_branch_name

# show log of commits affecting specific file
git whatchanged path/to/file

# display revisions and author for each line of a file (lines 450 - 470)
git blame -L 450,470 lib/file_name.rb

# view changes in commit, using SHA hash
git show 6d3b08115028d013d676bc03ece72db3e6e06225

# interactive rebase from remote master
git rebase -i origin/master

# create a branch from an earlier commit (time travel is possible!)
git checkout -b oldstuff commit_hash

# create a branch that tracks a remote branch
git checkout -b branch_name remote_name/branch_name

# merge selected files from another branch
git checkout frombranch thedir/thefile.txt anotherdir/anotherfile.txt

# undo an unsuccessful merge
git reset --hard

# undo a successful merge or commit
git reset --hard HEAD^

# delete a tracking branch
git branch -r -d otherguys/master

# update the last commit message
git commit --amend -m "New message"

# update the last commit with current date/time
git commit --amend --reset-author

# apply changes from local commit to current branch
git cherry-pick 04567899ae36651daf3dfa117a1088d594632370

# view configured remote repositories
git remote -v

# add remote repository
git remote add johndoe https://github.com/johndoe/myproject.git

# create local branch from remote branch
git checkout -b local_branch_name johndoe/remote_branch_name

# remove remote repository
git remote rm johndoe

# search for all commits (any branch) by message
git log --all --grep="contents of message"

# view commits in oneline format
git log --pretty=oneline

# view last 20 logs in reverse with raw comment body only - good for reporting work performed
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

# show changes between two branches
git diff master my-feature-branch

# show changes using time ranges
git diff --since=1.week.ago --until=1.minute.ago

# show commit SHA, author, and date for changes to file
git blame index.html --date short

# setup and use alias for complex git commands
git config --global alias.mylog "log --pretty=format:'%h %s [%an]' --graph"
git mylog

# create patch based on single commit
git format-patch -1 73699d42 --stdout > mycommit.patch

# create patch file (auto generated name) for current feature branch, using remote master as base
git format-patch origin/master

# check for errors before apply patch
git apply --check file.patch

# inspect / view statistics for patch
git apply --stat file.patch

# apply a patch
git am file.patch
```
