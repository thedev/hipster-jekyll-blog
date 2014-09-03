---
layout: post
title: Git notes
comments: true
tags:
- Tools, Git
---

GIT
Developed by Linus Torvalds after BitKeeper free of charge status revoked.
BitKeeper was used for Linux kernel maintainance.

Goals:
 speed
 simple
 strong support for non linear dev (branches)
 fully distributed
 able to handle large projects

Basics
 snapshots not differences
 most operations are local => speed, can work offline
 integrity, SHA-1 checksums
 only adds data, dont loose anything
 3 stages: commited, staged, modified
 working dir, staging area, git dir or repo



clone into other dir
$ git clone git://github.com/schacon/grit.git mydir

tracked and untracked files

git log --since=2.weeks

gitk
pull = fetch + merge

tagging:
lightweight vs annotated

setup git autocompletion

branching
 
workflow
- beacuse of 3 way merging we can have multiple brances always open for different stages

master - only realeased
develop or next = not necesarly stable but when it is can be merged to master, used to pull in topic branches and make sure they dont introduce any bugs
topic
proposed or pu

Topic branches
- usefull in any project
- short lived for a single particular work

Remote branches
local branches which are references to the state on the remote repo
automaticaly updated when fetching or pulling

Pushing
local branches not auto syncronized, need to specify
git push (remote) (branch)
git push origin serverfix:serverfix- to push local branch to remote branch with different name

git fetch origin - updates remote branches, but these are not editable (origin/serverfix)
in order to be able to work on severfix we can checkout a branch based on the remote
git checkout serverfix origin/serverfix

checking out a branch based on a remote branch creates a tracking branch (has direct relationship to remote branch, git automatically know to pull and push)

Deleting remote branch(obtuse)
git push [remotename] :[branch]
eg:
$ git push origin :serverfix
To git@github.com:schacon/simplegit.git
 - [deleted]         serverfix


Rebasing:
- GIT has 2 ways to integrate changes from one branch to another: merging and rebasing
With the rebase command, you can take all the changes that were committed on one branch and replay them on another one.
rebasing makes for a cleaner history
More Interesting Rebases
$ git rebase --onto master server client
This basically says, “Check out the client branch, figure out the patches from the common ancestor of the client and server branches, and then replay them onto master.”
Do not rebase commits that you have pushed to a public repository.

Git on the server
 remote repository is generally a bare repository — a Git repository that has no working directory.the contents of your project’s .git directory and nothing else.

check whitespace
git diff --check 


