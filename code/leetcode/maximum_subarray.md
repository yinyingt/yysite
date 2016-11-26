---
layout: post
title: LeetCode - Maximum Subarray
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/maximum-subarray/), or as follows: 

<pre>
Find the contiguous subarray within an array (containing 
at least one number) which has the largest sum.

For example, given the array [−2,1,−3,4,−1,2,1,−5,4],
the contiguous subarray [4,−1,2,1] has the largest sum = 6.

More practice:
If you have figured out the O(n) solution, try coding 
another solution using the divide and conquer approach, 
which is more subtle.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/maximum_subarray_medium/Solution1.py)

A brute-force solution of enumerating all possible combinations will work, but it has jarring run-time complexity of O(N!)! We can do better than that. 

This is a typical problem with a typical approach. The approach in Solution 1 is to maintain a local max and global max. Local max subarray is the subarray with max sum __containing the current element__, while global max one is the max sum we try to find by the end. The key is that the local max sum of this iteration is the __max(current element, previous local max sum + current element)__. Think about it: if the current element is larger than the sum of previous local max (should be negative in this case) and itself, the continous subarray should begin with this current element. Otherwise, add the current element on top of the previous local max sum. Of course, global max sum should be compared and updated (if necessary) every time. This is also a greedy algorithm. Run-time complexity is O(N), and space complexity is O(1). Compared with the O(N!) brute-force solution, life is much better. 

A extension of this problem is ["Maximum Product Subarray"](./maximum_product_subarray.html) problem.
