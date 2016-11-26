---
layout: post
title: LeetCode - 3 Sum Closest
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/3sum-closest/), or as follows: 

<pre>
Given an array S of n integers, find three integers 
in S such that the sum is closest to a given number, 
target. Return the sum of the three integers. You may 
assume that each input would have exactly one solution.

For example, given array S = {-1 2 1 -4}, and target = 1.

he sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/3_sum_closest_medium/Solution1.py)

This is not so different from ["3 Sum"](./3_sum.html) problem except for the additional logic to update the global minimum difference between the 3-sum and the target. 

