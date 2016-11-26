---
layout: post
title: LeetCode - Search for A Range
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/search-for-a-range/), or as follows: 

<pre>
Given a sorted array of integers, find the starting 
and ending position of a given target value.

Your algorithm's runtime complexity must be in the 
order of O(log n).

If the target is not found in the array, return 
[-1, -1].

For example,
Given [5, 7, 7, 8, 8, 10] and target value 8,
return [3, 4]. 
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/search_for_a_range_medium/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/search_for_a_range_medium/Solution2.py)

First of all, the requirement of O(log(N)) running time complexity indicates a binary search algorithm. 

Solution 1 is easy to think: first find the target value in the array by binary search, and linearly search for its lower and upper bound. However, the worst case can be O(N) in running time, when all elements in the array equals the target value. 

Solution 2 is a more "binary search" flavored algorithm. It first searches for the target value with binary search, and then search for the lower and upper bounds of the range with binary search. Pay attention to how the binary search code for lower and upper works. 

Clearly, Solution 2 is the winner here. 
