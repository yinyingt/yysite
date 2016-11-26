---
layout: post
title: Quick Reference of Python
mathjax: false
related: false
comments: true
published: true
---

_Last modified: Aug, 2016_

This page is for Python in general. There are also specific pages for [numpy, scipy](./numpy_scipy.html) and [matplotlib](./matplotlib.html).

## Infinity number in Python

Use this 

{% highlight python %}
float("inf")
{% endhighlight %}

## Sort a list of list/tuples by certain position

Below is an example of sorting a list of tuples by the 2nd item

{% highlight python %}
>>> from operator import itemgetter
>>> data = [('a', 10),('b', 7),('c', 1), ('d',9)]
>>> sorted(data,key=itemgetter(1))
[('c', 1), ('b', 7), ('d', 9), ('a', 10)]
{% endhighlight %}

Reference: 

* [Python wiki: Sorting Mini-HOW TO](https://wiki.python.org/moin/HowTo/Sorting#Key_Functions)
* [A discussion on StackOverflow](http://stackoverflow.com/questions/10695139/sort-a-list-of-tuples-by-2nd-item-integer-value)


## Virtualenv

A [virtualenv](http://virtualenv.org/) environment will have access to global (system-level) Python packages outside but keep the local packages separately. So it is a good idea to minimize the installation of global packages for minimal dependency hell. 

* To create and activate a virtualenv environment on Linux: 

```
sudo apt-get install python-virtualenv  # works on Ubuntu/Debian
virtualenv my_env
source my_env/bin/activate
```

After this, `which python` and `which pip` should give local Python and Pip instances. 

* To create and activate a virtualenv environment on Windows with Anaconda which comes with virtualenv by default) and MINGW shell: 

```
virtualenv my_env
source my_env/Scripts/activate
```

* (Optional) Upgrade pip first:

```
pip install --upgrade pip
```

* Installing with the "requirements" feature of Pip can be useful when configuring a new virtualenv container: 

```
env/bin/pip install -r requirements.txt
```

Refer to [Pip user guide](https://pip.pypa.io/en/latest/user_guide/) and [requirement file format](https://pip.pypa.io/en/latest/reference/pip_install/#requirements-file-format) for more information.


## JSON-related

* Get JSON data after certain inquiry: 

```
import urllib2
import simplejson

# Search Kickstarter with a keyword
url = 'http://www.kickstarter.com/projects/search.json?search=&term=USB'
res = urllib2.urlopen(url) 
data = simplejson.load(res)
print data # will be a dictionary
```

## Generators

[This is such a good article](https://jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/) that explained in an out of generators and keyword `yield` in Python.

Another highly upvoted [StackOverflow post](http://stackoverflow.com/a/231855) on what Python generator is and what keyword `yield` does.

## Traps

Some "traps" below seem "naive" but caused lots of head-scratching

* When testing the existence of a key in a dictionary, do: 

```
if key in dictionary:
```

but not this 

```
if key in dictionary.keys():
```

because the latter will generate an intermediate array and can be very slow if the dictionary has lots of keys.
