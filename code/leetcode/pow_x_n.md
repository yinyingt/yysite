---
layout: post
title: LeetCode - Pow(x, n)
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/powx-n/), or as follows: 

<pre>
Implement pow(x, n).
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/pow_x_n_medium/Solution1.py)


While, technically, a naive approach like mutiply x by n times does the job (run-time complexity of O(N)), it is not very efficient if n is very large. More efficient way with O(log(N)) best case and O((log(N))^2) worst case run-time complexity starts with decompose n to the sum of 2^(2^(k-1)) series (with k>=1), while k is an integer. The idea is to let the product rolls itself over exponentially rather than linearly. For example, if n is 23, then it can be decomposed to 

{% highlight bash %}
23 = 16 + 4 + 2 + 1
{% endhighlight %}

so that the total number of multiplication operations to get the result is (4+2+1+0) +3 = 10 (the additional 3 is needed to multiply four terms together).

Again, several situations needed to consider: 

* What if x is negative?
* What if n is negative?
* What if abs(x) is smaller than 1?
* What if the result is too large (int type overflow)?

See the code for details. This algorithm is a common approach to implement some _advanced_ numerical computation. Another example is ["Divide Two Integers"](./divide_two_integers.html) problem. 
