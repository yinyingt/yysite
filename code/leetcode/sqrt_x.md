---
layout: post
title: Leetcode - Sqrt(x)
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/sqrtx/), or as follows: 

<pre>
Implement int sqrt(int x).

Compute and return the square root of x.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/sqrt_x_medium/Solution1.py)

Note that here the return type is int, meaning it will round up to the maximum integer smaller or equal to sqrt(int x). 

Essentially, Solution 1 tries to solve the y^2 = x equation (where y is the return value) with binary search, and the initial range for y is [0, x]. Note that the matching condition is not mid^2==x, but mid^2<= x and (mid+1)^2>x (here ^ is power operator, not XOR). 

