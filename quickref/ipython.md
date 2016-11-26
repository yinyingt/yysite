---
layout: post
title: Quick Reference of IPython
mathjax: false
related: false
comments: true
published: true
---

_Last modified: Feb, 2014_

## General info

* [IPython in-depth Tutorial, first presented at PyCon 2012](https://github.com/ipython/ipython-in-depth)

## Installation

The installation guide is [here](http://ipython.org/install.html). In Ubuntu, it is as simple as 

```
sudo apt-get install ipython
```

## Debugging with IPython

The module __ipdb__ (IPython-enabled pdb) offers extra features that are not available in __pdb__. As of Ubuntu 12.04, ipdb is not in the repository. But it can be easily installed by pip

```
sudo apt-get install python-pip
sudo pip install ipdb
```

To insert a breakpoint to the code is as easy as 

{% highlight python %}
import ipdb; ipdb.set_trace()
{% endhighlight %}

For example, an example Python script with name "tmp.py" and content as follows:

{% highlight python %}
import ipdb

for n in range(5):
    print "Breakpoint below!"
    ipdb.set_trace()
    print n
{% endhighlight %}

After executing the script 

```
python tmp.py
```

it will enter ipdb debugging mode. There are several shortcut keys for debugging, such as "__c__" for "continue", "__n__" for "next", "__q__" for quitting debugging mode, etc. 

Some useful posts on debugging with IPython

* [Debugging Python in ipython and ipdb](http://shanit.blogspot.com/2010/06/debugging-python-with-ipython-and-ipdb.html)
