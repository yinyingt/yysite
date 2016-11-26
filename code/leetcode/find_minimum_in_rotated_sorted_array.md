---
layout: post
title: LeetCode - Find Minimum in Rotated Sorted Array
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/find-minimum-in-rotated-sorted-array/), or as follows: 

<pre>
Suppose a sorted array is rotated at some pivot 
unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

You may assume no duplicate exists in the array.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/find_minimum_in_rotated_sorted_array_medium/Solution1.py)

It is another twist of [Search in rotated sorted array I and II](./search_in_rotated_sorted_array_I_and_II.html). The algorithm is probably easy to come about: if the subarray is in ascending order (left element smaller than middle element smaller than right element), then return the left element. If not, then set the search bounds to the rotated part (either left subarray or right subarray). Some details need to be taken care of. See the code. 
