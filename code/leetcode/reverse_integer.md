---
layout: post
title: LeetCode - Reverse Integer
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/reverse-integer/), or as follows: 

<pre>
Reverse digits of an integer.

Example1: x = 123, return 321
Example2: x = -123, return -321 
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/reverse_integer_easy/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/reverse_integer_easy/Solution2.py)

The idea is to think the input and output as two pipelines, where input pipeline is left-in-right-out, and output pipeline is right-in-left-out. In each iteration, pull a digit out from the right of the input, and feed it in from the right of the output. Shift the input and output both by a digit. In C++ or Java, there is another potential problem of the int type overflow if the number is too large or too small, while it is there for Python. So in Solution 1, I actually have to fake the 32-bit int type overflow in order to pass the LeetCode OJ engine. 

Solution 2 is similar, but cleaner. 
