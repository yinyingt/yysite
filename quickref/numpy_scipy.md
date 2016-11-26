---
layout: post
title: Quick Reference of NumPy and SciPy
mathjax: false
related: false
comments: false
published: true
---

_Last modified: Jun, 2014_


## General info

* NumPy functions on [sorting, searching and counting](http://docs.scipy.org/doc/numpy/reference/routines.sort.html)


## Linear regression

Both NumPy and SciPy have functions for simple linear regression (see [post](http://glowingpython.blogspot.com/2012/03/linear-regression-with-numpy.html)). It is more intuitive to do in SciPy as follows: 

{% highlight python %}
from scipy import stats

x = [1, 2, 3, 4]
y = [2, 4, 7, 9]

slope, intercept, r_value, p_value, std_err = stats.linregress(x,y)
{% endhighlight %}

The meaning of each output is self-explanatory in the code above. 

## Find indices of an array

The "find" function in Matlab is very handy, but it doesn't have a direct clone in NumPy. 

* Most commonly, function [numpy.where](http://docs.scipy.org/doc/numpy/reference/generated/numpy.where.html#numpy.where) is the closest one to the "find" in Matlab: 

{% highlight python %}
import numpy as np 

a = np.array([6,7,8,9])
print np.where((a > 6) & (a <9))
{% endhighlight %}

will return a tuple "(array([1, 2]),)" which is the array of matched indices. 

* Or, a custom function mimicking the find in Matlab should great. 

Related functions on sorting, searching and counting are [here](http://docs.scipy.org/doc/numpy/reference/routines.sort.html).

## Find the size of a matrix 

This is equivalent to the "size" function in Matlab:
{% highlight python %}
(m, n) = mtx.shape
{% endhighlight %}

where "mtx" is a matrix. 

However, there is a caveat: if the matrix is an 1D array, then the returned tuple may miss the corresponding "1". For example:

{% highlight python %}
import numpy as np 
a = np.linspace(1,10, 10)
print a.reshape(5,2)[:,0].shape
{% endhighlight %}

may just return (5,) instead of (5,1). This is the difference from the "size" function in Matlab.

## NumPy for Matlab Users

http://wiki.scipy.org/NumPy_for_Matlab_Users

## Read Text Tables in Python

http://penandpants.com/2012/03/09/reading-text-tables-with-python/