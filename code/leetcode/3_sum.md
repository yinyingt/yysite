---
layout: post
title: LeetCode - 3 Sum
mathjax: false
related: false
comments: true
---


_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/3sum/), or as follows: 

<pre>
Given an array S of n integers, are there elements a, 
b, c in S such that a + b + c = 0? Find all unique 
triplets in the array which gives the sum of zero.

Note:
* Elements in a triplet (a,b,c) must be in 
non-descending order. (ie, a ≤ b ≤ c)
* The solution set must not contain duplicate triplets.

For example, given array S = {-1 0 1 2 -1 -4}, a 
solution set is:
    (-1, 0, 1)
    (-1, -1, 2)
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/3_sum_medium/Solution1.py)

Again, this can be done with either with hash table, but it became complicated in logic when dealing with repeated elements in the array, and the code exceeded running time limit on LeetCode online judge engine. 

On the other hand, three-sum problem can be thought of two-sum problem embedded in another two-sum problem. The solution in last ["Two Sum"](./two_sum.html) can be used as a subroutine here. 
