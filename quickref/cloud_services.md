---
layout: post
title: Cloud Services Notes
mathjax: false
related: false
comments: true
published: true
---

_Created: Oct, 2015_

_Last modified: Oct, 2015_



## Work with Openshift

The basic idea is quite simple: there will be a private git repository on Openshift that the local client to push to. A "git push" from the local to Openshift will trigger the deployment of the new code. 

* Install "rhc" first: 

```
sudo gem install rhc
```

* Run 

```
rhc setup 
```

if setting up Openshift client for the first time. 

* Create a Nodejs app by 

```
rhc create-app vidzy nodejs-0.10
```

where "vidzy" is the app name on Openshift. It will create a remote git repository called "vidzy" on Openshift which is subsequently cloned to local computer with the same name. For future reference, do 

```
rhc show-app vidzy
```

to show the app information. 

* Add MongoDB by adding a cartridge (a modularized add-on service provided by Openshift in additional the to core application like Nodejs, Python, etc.) 

```
rhc cartridge add mongodb-2.4 -a vidzy
```

which means adding a MongoDB (version 2.4) database service to the app named "vidzy" on Openshift. There will be credential information in the output. Write it down.

It is also helpful to administrate the database via web interface. Openshift provides RockMongo cartridge to do this after the MongoDB cartridge is installed: 

```
rhc cartridge add rockmongo -a vidzy
```

After that the database can be adminstrated via link http://appname-domainname.rhcloud.com/rockmongo.

* One can also integrate Github to track the same source by adding a remote Github repository, such as

```
git remote add github git@github.com:username/repo_name.git
```

But before doing any work, fetch and merge the code on Github with the local branch first so they have the same starting point. Also see [this post](http://stackoverflow.com/questions/12657168/can-i-use-my-existing-git-repo-with-openshift) for related discussion.

* To get the debugging log from an app, do 

```
rhc tail -a vidzy
```

* It seems Openshift always starts the app from "server.js", which the new ExpressJS 4 template doesn't have. Try [this modified Express 4 template](https://github.com/master-atul/openshift-express4). 


