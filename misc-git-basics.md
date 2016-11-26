---
layout: post
title: Basic Git Commands
mathjax: false
related: false
comments: false
published: true
---




## Concepts

* Think of git system like a warehouse. There are three places each file can be: being outside the warehouse ("untracked"), being in the warehouse but still in loading zone ("staged"), being organized and put on shelves ("committed").  (Well, that is just my understanding...). 

## Basic configuration 

* One must to set the username and password before being able to commit. One can do it system-wide like

```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

or only set this information locally to a specific repository

```
git config user.name "John Doe"
git config user.email johndoe@example.com
```

If same records exist in both global and local settings, local ones are prioritized.

* Set the editor for editting the commit message: 

```
git config --global core.editor "vim"
```

or set "export GIT\_EDITOR=vim" in "~/.bashrc/".


## Staging and committing

* Check file status

```
git status
```

* Add untracked files to stage, or update tracked but modified files to stage

```
git add [files]
```

A short-hand 

```
git add . 
```

will stage all files under current directory

* Commit 

```
git commit -m 'commit message'
```

or just

```
git commit
```

followed by inputing the commit message interactively. 

* Another short-hand skipping `git add`: 


```
git commit -a -m 'commit message'
```


* Move file 

```
git mv file_from file_to
```


## Viewing changes 

* View what is changed but still not stage

```
git diff 
```

* View what is staged that will go to the next commit

```
git diff --cached
```
 
or 

```
git diff --staged
```

## Viewing git log

* Most general command

```
git log
```

* Show the difference introduced in last 2 commits

```
git log -p -2
```

* Show abbreviated diff stats

```
git log --stat
```


* The `--pretty` flag formats the `git log` output. For example, to format the `git log` output in form of one line for each commit

```
git log --pretty=oneline
```

* Show branching and merging history in ASCII-art like style using `--graph` flag

```
git log --pretty=format:"%h, %s" --graph
```

The meaning of `%h` and `%s` can be found below:

```
Option  Description of Output
%H  Commit hash
%h  Abbreviated commit hash
%T  Tree hash
%t  Abbreviated tree hash
%P  Parent hashes
%p  Abbreviated parent hashes
%an Author name
%ae Author e-mail
%ad Author date (format respects the –date= option)
%ar Author date, relative
%cn Committer name
%ce Committer email
%cd Committer date
%cr Committer date, relative
%s  Subject
```

* Show commit log in last two weeks

```
git log --since=2.weeks
```

and some useful option flags:

```
Option  Description
-(n)    Show only the last n commits
--since, --after    Limit the commits to those made after the specified date.
--until, --before   Limit the commits to those made before the specified date.
--author    Only show commits in which the author entry matches the specified string.
--committer Only show commits in which the committer entry matches the specified string.
```

* Show tree structure with "git log" command

```
git log --graph --decorate --all
```

* A GUI tool to view the `git log` is

```
gitk
```


## Undoing things

* Change the last commit, such as adding a forgotten file, editing the commit message, etc.

```
git commit --amend
```

That is convenient: it allows you to commit frequency, and in the end it can just be commit if using this amend command after each commit. An example will be

```
git commit -a -m 'update'
git add forgotten_file
git commit --amend -m 'update2'
git add another_forgotten_file
git commit --amend 'update3'
```

And in the end there will just be one commit in the log with commit message "update3". 


* Change author in an amended commit

```
git commit --amend --reset-author 'update4'
```

* Revert a 'git add' before 'git commit', that is, unstage files that are already staged: 

```
git reset HEAD [files]
```

which will leave the file as "modified". To even discard the changes made since last commit, that is, unmodify modified files:

```
git checkout -- [files]
```

But "git checkout" will not remove the untracked files and directories. Use "git clean" to remove the untracked ones, do 

```
git clean -f    # remove untracked files
git clean -df   # remove untracked directories
```

See [the documentation for "git-clean"](https://git-scm.com/docs/git-clean) for details. 


## Rolling back to previous commits

First check out a commit from the history:

```
git checkout [commit] . 
```

Note that the period "." in the end means apply the changes to the current stage. 

If only need to check out a specific file of a specific commit, do: 

```
git checkout [commit] [file-path]
```

And then commit the changes and make a new commit: 

```
git add . 
git commit -m "message"
```


## Removing and moving files

* Remove files both from git stage and physical hard drive

```
git rm [files]
```

* Remove files only from git stage, but still keep it on physical hard drive

```
git rm --cached [files]
```

which will leave files as "untracked". 


## Working with remotes

* Show remotes

```
git remote
```

* Add a remote

```
git remote add [remote_nickname] [remote_url]
```

such as 

```
git remote add origin git@github.com:vadiode/git_notes.git
```

* Show information about a remote

```
git remote show [remote_nickname]
```

or with flag `-v`

```
git remote show [remote_nickname] -v
```

* Rename a remote

```
git remote rename [old_remote_name] [new_remote_name]
```

Edit the remote url

```
git remote set-url origin new_url
```

Git remote with a custom SSH port (other than 22): edit ~/.ssh/config like this 

```
Host example.com
Port 1234
```

* Remove a remote

```
git remote rm [remote_name]
```

* Push a local branch to remote branch

```
git push [remote_name] [local_branch_name]:[remote_branch_name]
```

If the local branch and remote branch have the same names, this command can be shortened as: 

```
git push [remote_name] [branch_name]
```




## Tagging

* Tags can be either lightweight (just a tag name), or annotated (with tagging message).  If it is annotated, it can be even signed with GPG.  

* A newly created tag will not be pushed to remote by default. A `git push` is needed if you want to do that. 

* Show existing tags

```
git tag
```

* Show existing tags with a pattern (using wildcards)

```
git tag -l 'v1.4.*'
```

* Show tag information

```
git show [tag_name]
```

such as 

```
git show v1.4.0
```

* Apply a lightweight tag (with no tagging message) to the current commit

```
git tag v1.4.1-lw
```

* Apply an annotated tag to the current commit

```
git tag -a v1.4.1 -m 'version 1.4.1, finished in 12/11/2011'
```

* Sign a tag with GPG, using `-s` flag

```
git tag -s -a v1.4.1 -m 'version 1.4.1, finished in 12/11/2011, with signature'
```

* Tag a specific commit

```
git tag -a [tag_name] [commit_hash]
```

such as

```
git tag -a v1.4.1 9fceb02
```

* Push a tag to remote

```
git push [remote_name] [tag_name]
```

such as

```
git push origin v1.4.1
```

* Push all tags to remote

```
git push origin --tags
```

## Useful Git Commands

* Compare changed files between two commits. See [this post](http://stackoverflow.com/a/1552353).

```
git diff --name-only SHA1 SHA2
```

Omitting "--name-only" flag will make it show the diff of file content.
