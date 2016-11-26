---
layout: post
title: Quick Reference of Matplotlib
mathjax: false
related: false
comments: false
published: true
---





_Last modified: Oct, 2015_


## General info

* [The very first tutorial](http://matplotlib.org/users/pyplot_tutorial.html)
* [Use TeX for text rendering](http://matplotlib.org/users/usetex.html)
* [Matplotlib VS Pyplot VS Pylab](http://matplotlib.sourceforge.net/faq/usage_faq.html#matplotlib-pylab-and-pyplot-how-are-they-related)


## A typical plotting script using Matplotlib

This script tries to do the similar task with the [example script in Matlab](./matlab.html). 

{% highlight python %}
import numpy as np
import matplotlib.pyplot as plt

#plt.close()
plt.figure(1, figsize=(10,8))   # figure size 10" by 8"
ax = plt.subplot(111)
x = np.linspace(0,2*np.pi, 100) 
y1 = np.sin(x)
y2 = np.cos(x)
lines = plt.plot(x, y1, 'b-', x, y2, 'r--')
plt.setp(lines, 'linewidth', 3)
plt.xlabel('x')
plt.ylabel('y')
plt.xlim([0, 2*np.pi])
plt.ylim([-2, 2])
ax.xaxis.label.set_fontsize(18)   # set x-axis label font size 
ax.yaxis.label.set_fontsize(18)   # set y-axis label font size 
for item in (ax.get_xticklabels() + ax.get_yticklabels()):
    item.set_fontsize(18)   # set axis tick font size
plt.legend(['sin(x)', 'cos(x)'], loc = 'lower left', prop={'size':'20'})   # loc: legend location
plt.show()
{% endhighlight %}


## Plot multiple y axes/scales in a single plot

See the official example [here](http://matplotlib.org/examples/api/two_scales.html), using ax.twinx(). My example: 

{% highlight python %}
plt.figure(1, figsize=(10, 8))
ax = plt.subplot(111)
plt.plot(t1, f1)
plt.xlabel('t')
plt.ylabel('f')
plt.ylim([0, 1000])
plt.legend(["f1"], loc="upper left")
ax.xaxis.label.set_fontsize(18)   # set x-axis label font size
ax.yaxis.label.set_fontsize(18)   # set y-axis label font size
for item in (ax.get_xticklabels() + ax.get_yticklabels()):
    item.set_fontsize(18)   # set axis tick font size

ax2 = ax.twinx()
plt.plot(t1, T1, '--r')
plt.legend(["T"], loc="upper right")
plt.ylabel('T')
plt.ylim([-5, 60])
ax2.xaxis.label.set_fontsize(18)   # set x-axis label font size
ax2.yaxis.label.set_fontsize(18)   # set y-axis label font size
for item in (ax2.get_xticklabels() + ax2.get_yticklabels()):
    item.set_fontsize(18)   # set axis tick font size
{% endhighlight %}


## Import data

* Alternative to the "importdata" function in Matlab, Numpy has "loadtxt". For exapmle: 

{% highlight python%}
import numpy as np
data = np.loadtxt('data.dat', skiprows=1)  # skip the first row during the import
{% endhighlight %}

## Add text to plots

Usually there are two ways: 

1. Use matplotlib.pyplot.text(xpos, ypos, string). For example: 

{% highlight python %}
import numpy as np
import matplotlib.pyplot as plt

plt.text(1, 2, 'this is text')
{% endhighlight %}

Extra manipulation such as text wrapping can be found [here](http://stackoverflow.com/questions/4018860/text-box-in-matplotlib). 

2. Use matplotlib.pyplot.annotate. Refer to [this link](http://matplotlib.org/users/annotations_intro.html). 

## Use TeX for rendering text

* Enable TeX rendering by 

{% highlight python%}
import matplotlib.pyplot as plt
plt.rc('text', usetex=True)
{% endhighlight %}

## Working with directory

Sometimes one needs to import many data files to the workspace and plot. This can be realized with the help of package "glob" which is capable of matching wildcards in path patterns. For example, 

{% highlight python%}
import numpy as np 
import matplotlib.pyplot as plt
import glob

data_files = glob.glob('data*.dat') 
# data_files will be a string array containing all files matching 'data*.dat' pattern

plt.figure(1)
for d in data_files: 
    data = np.loadtxt(d)
    plt.loglog(data[:,0], data[:,1])  # assuming data is a 2D array
plt.show()
{% endhighlight %}
