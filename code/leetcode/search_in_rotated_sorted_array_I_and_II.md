---
layout: post
title: LeetCode - Search in Rotated Sorted Array (I and II)
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem I description is [here](https://oj.leetcode.com/problems/search-in-rotated-sorted-array/), or as follows: 

<pre>
Suppose a sorted array is rotated at some pivot 
unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

You are given a target value to search. If found 
in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/search_in_rotated_sorted_array_medium/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/search_in_rotated_sorted_array_medium/Solution2.py)

This is an interesting twist of binary search. The idea is not very different from a conventional binary search. However, the situation is a little complicated in subcases because of the possibility of rotated array, and it may appear subtle. 

Solution 1 follows a conventional thread, discussing the situations when middle item is larger (or smaller) than the target value. In Solution 2, it starts with whether the middle item is larger (or smaller) than the right end item, so that to distinguish and operate on the subarray that is not rotated. Both solution shares very similar code but are different in relational logic. The running time complexity is still O(log(N)). 

To me, Solution 1 is more straightforward. But the logic of Solution 2 is easier to follow for the extended problem below (allowing duplicate elements). 

A slightly modified problem allowing duplicate elements is [here](https://oj.leetcode.com/problems/search-in-rotated-sorted-array-ii/), or as follows:

<pre>
Follow up for "Search in Rotated Sorted Array":
What if duplicates are allowed?

Would this affect the run-time complexity? How 
and why?

Write a function to determine if a given target is 
in the array.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/search_in_rotated_sorted_array_II_medium/Solution1.py)

This solution is based on the [Solution 2](https://github.com/lijunhw/leetcode_practice/blob/master/search_in_rotated_sorted_array_medium/Solution2.py) of the Problem I. However, the change of code is minimal: it only needs an extra case in which middle element equals the right end element. In that case, all elements in the subarray between the middle element and right end element have the same value. So jump out of the homogeneous subarray, make right end index back off by 1 each time until the middle element does not equal the right end element. Obviously, the run-time complexity is changed, resulting in O(N) in the worst case. 
