---
layout: post
title: Git Branching and Merging
mathjax: false
related: false
comments: false
published: true
---

## Concepts

* Except for the first commit, each commit has a "parent" pointer that points to its nearest predecessor ("parent") commit. 

* Creating a branch is simply creating a pointer that points to a commit. 

* Pointer "HEAD" is a special pointer that points to the current branch pointer. It keeps tracking which branch one is currently working on. 

* Merging two branches in git is a three-way process. It first creates a new commit that points to the very last commits of two branches to be merged, resolves the merging conflicts starting from the latest common ancestor commit if there is any, and merge. See [Fig 3-17 in Chapter 3.2](http://git-scm.com/figures/18333fig0317-tn.png)

* It is a good practice to create branches with several levels of stabilities. For example, let branch "master" be the most stable one (major releases), and branch "development" be the next-release work, and branch "topic" be short-lived experimental one branched from "development", and so on. 

* The process of rebase (I quote here)
```
It works by going to the common ancestor of the two branches (the one you are on  
and the one you are rebasing onto), getting the diff introduced by each commit of the  
branch you are on, saving those diffs to temporary files, resetting the current branch to  
the same commit as the branch you are rebasing onto, and finally applying each change  
in turn.
```


## Branching and basic merging

* View branches in current repository

```
git branch
```

and the branch with "*" in the beginning is the branch you are currently on. 

```
git branch -v
```

will show more information such as hashed string and commit comments

* View the branches that have been merged: 

```
git branch --merged
```

* View the branches that haven't been merged: 

```
git branch --no-merged
```

* Create a branch called "experiment": 
```
git branch experiment
```

* Switch to branch "experiment": 
```
git checkout experiment
```

This basically lets pointer "HEAD" points to the master branch pointer. 

* Create a branch and switch to it:
```
git checkout -b experiment
```

which is equivalent to 

```
git branch experiment  
git checkout experiment
```

* Now merge branch "experiment" back into master, you need first switch to "master" and merge "experiment" branch in: 

```
git checkout master  
git merge experiment
```

* Remove branch "experiment" (after merging into master):

```
git branch -d experiment
```

* Check out a previous commit (e.g., with hash "123456789abcdef"), bash to a new branch

```
git checkout -b new_branch 123456789abcdef
```


## Merging conflict

* A merging conflict may occur if two branches modify the same file in different ways. Git won't create new commit until you resolve the merging conflict. It will be shown as "unmerged" after ```git status```. 

* To resolve the conflict, you need to make changes to the conflict file in two branches and make they have the same content. 

* Some visualizer git merging tools may help facilitate the process: 

```
git mergetools
```

## Remote branches

* Remote branches are referred as ```[remote_name]/[remote_branch_name]``` in git, such as "origin/master". 

* Fetch all branches from remote "origin"

```
git fetch origin
```

or, just fetch a specific branch "master" from remote "origin"

```
git fetch origin master
```

* Remember that a ```git fetch origin master``` will not modify the local "master" branch. They may contain different commits. 

* And a ```git fetch origin master``` doesn't create an editable local copy of "origin/master" automatically. To make a local editable copy of "origin/master": 

```
git checkout -b master_from_origin origin/master
``` 

This create a so-called _tracking branch_ of "origin/master". Changes made to local branch "master_from_origin" can be in sync with "origin/master" directly via ```git push``` and ```git pull```. 

* Push a local branch to remote

```
git push [remote_name] [local_branch_name]:[remote_branch_name]
```

such as 

```
git push origin master_from_origin:master
```

A special case is if the local branch name and remote branch name are the same

```
git push origin experiment
```

is equivalent to 

```
git push origin experiment:experiment
```

* Remove remote branch "origin/experiment"


```
git push origin :experiment
```

It is like saying "go and take an empty local branch and push it to origin/experiment" (i.e. "delete origin/experiment").


## Rebasing

* Rebasing is another way to integrate changes from another branch, but should be used with caution if *working with a public repository". 

* Rebasing a branch "experiment" onto "master":

```
git checkout experiment  
git rebase master
```

It will create a new rebased commit in branch "experiment" that looks like a fast-forward commit of branch "master". And then you can merge with branch "master" with a fast-forward fashion:

```
git checkout master  
git merge experiment
``` 

The resulting branch will look like a clean straight-line branch "master".

* Perils of rebasing: * Don't rebase the commits you have pushed onto a public repository*, because it changes the history of past branch.
