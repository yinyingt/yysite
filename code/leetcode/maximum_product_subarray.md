---
layout: post
title: LeetCode - Maximum Product Subarray
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/maximum_product_subarray/), or as follows: 

<pre>
Find the contiguous subarray within an array (containing 
at least one number) which has the largest product.

For example, given the array [2,3,-2,4],
the contiguous subarray [2,3] has the largest product = 6.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/maximum_product_subarray_medium/Solution1.py)

The idea is very similar to the ["Maximum Subarray"](./maximum_subarray.html) problem (maintaining a local max and global max), but the situation is more complicated here because the a previous local minimum (if being negative) times the current element (if also being negative) can yield a local maximum. So what is different here is that we maintain local maximum product and local minimum product besides the global maximum product. Again, the definition of local maximum/minimum product is the maximum/minimum product containing the current element. And the recurrence relationships are as follows:

* local maximum product = max(previous local maximum product * current element, previous local minimum product * current element, current element)
* local minimum product = (previous local maximum product * current element, previous local minimum product * current element, current element)

Think about the above relationships. See if it makes sense if the current element is positive, negative, or zero. 

The rest part is the same to ["Maximum Subarray"](./maximum_subarray.html) problem. 
