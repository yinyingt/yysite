---
layout: post
title: "Use Simplenote as A Note-taking Tool"
mathjax: false
related: false
comments: true
published: true
---


_Created: Mar, 2016_

There are dozens of solutions for note-taking. In my opinion, [Simplenote](http://simplenote.com/) hits the sweep spot of usability VS simplicity. Besides the official apps for various platforms, there are also many [3rd-party clients or plugins](http://simplenote.com/downloads/). 

The support of Markdown is a big plus. Although there is no direct way of inserting images in Simplenote which can be very useful in many cases, I circumvent by creating an [Imgur](http://imgur.com/) account, and use the direct links in Markdown after uploading images to Imgur. 

(Note that one can register an Imgur account and make the https://acount_name.imgur.com/all/ private, although all images are still publicly accessible with direct links. Just don't upload images with sensitive information to Imgur. Images don't expire on Imgur until you delete them.)


## Use nvPY as a Simplenote desktop client

Maybe this desktop client is not the most beautiful one you can find, but it works, both in Linux (I am looking at you Evernote) and Windows!

The following works in Ubuntu 14.04: 

* First install tkinter as the dependency 

```
sudo apt-get install python-tk
```

And then install nvPY: 

```
sudo pip install nvpy
```

* Now create a config file in "~/.nvpy.cfg". For more configuration details, refer to this [README](https://github.com/cpbotha/nvpy/blob/master/README.rst) and this [configuration file template](https://github.com/cpbotha/nvpy/blob/master/nvpy/nvpy-example.cfg)

The same procedure above also works under Windows! I use Anaconda Python distribution, and use pip to install nvpy. 


## Use SyncPad in Chrome browser

[Syncpad (Updated) for Chrome](https://chrome.google.com/webstore/detail/syncpad-for-simplenote-up/pcchkpddpepanbahfbdhcnmfbkanibog) is a good solution to work with Simplenote in Chrome browser and Chromebook. This version fixed some bugs in the old SyncPad app. The best part is that one can take notes in a drop-down window when browsing a web page in Chrome browswer. Compared with the official app, one doesn't have to switch back and forth if needing some information for another web page. 

The UI is not beautiful but functioning well. 


## Interface Simplenote data in Python 

Although [Simplenote Developer page](simplenote.com/developers/) says the options for developers are not ready yet, one can still use a Python library called [simplenote.py](https://github.com/mrtazz/simplenote.py). The documentation is [here](https://simplenotepy.readthedocs.org/en/latest/index.html). The library is very easy to use, and can be useful for automated applications such as content backup. 
