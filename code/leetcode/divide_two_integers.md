---
layout: post
title: LeetCode - Divide Two Integers
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/divide-two-integers/), or as follows: 

<pre>
Divide two integers without using 
multiplication, division and mod operator.

If it is overflow, return MAX_INT. 
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/divide_two_integers_medium/Solution1.py)


The approach used in this problem is similar to ["Pow(x,n)"](./pow_x_n.html) problem. The idea is to match the quotient with the form of sum of 2^(2^(k-1)) series. See the code for details, and the ["Pow(x,n)"](./pow_x_n.html) problem for algorithm explanation. 
