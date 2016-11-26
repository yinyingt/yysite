---
layout: post
title: LeetCode - Search Insert Position
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/search-insert-position/), or as follows: 

<pre>
Given a sorted array and a target value, return 
the index if the target is found. If not, return 
the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Here are few examples.
[1,3,5,6], 5 → 2
[1,3,5,6], 2 → 1
[1,3,5,6], 7 → 4
[1,3,5,6], 0 → 0 
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/search_insert_position_medium/Solution1.py)

This is a typical binary search problem, but is also an important building block for binary search problem variants. It will be useful to memorize this solution. Also see the discussion [here](http://blog.csdn.net/linhuanmars/article/details/20278967). 


