---
layout: post
title: LeetCode - Find Peak Element
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/find-peak-element/), or as follows: 

<pre>
A peak element is an element that is greater than 
its neighbors.

Given an input array where num[i] ≠ num[i+1], find a 
peak element and return its index.

The array may contain multiple peaks, in that case 
return the index to any one of the peaks is fine.

You may imagine that num[-1] = num[n] = -∞.

For example, in array [1, 2, 3, 1], 3 is a peak element 
and your function should return the index number 2.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/find_peak_element_medium/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/find_peak_element_medium/Solution2.py)

Solution 1 is a naive solution with run-time complexity of O(N) that everyone can think of: basically iterate the whole array and detect the peak. 

A smarter solution is presented in Solution 2 using binary search. The question is how to apply binary search, which is a global search algorithm, to searching for a local maximum. The idea is to compare the middle element with the left adjacent element. If the middle element is larger than the left adjacent element, it means there should be a peak element to the right of the middle element. So one should set the left search bound to the middle index. Otherwise, there should be a peak element to the left of the middle element; thus shrink the right search bound to the middle index. This is how to combine the global search with local search. 

Solution 2 is interesting because it demonstrates binary search does not have to look alike.
