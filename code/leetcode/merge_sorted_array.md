---
layout: post
title: LeetCode - Merge Sorted Array
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/merge-sorted-array/), or as follows: 

<pre>
Given two sorted integer arrays A and B, merge B into A 
as one sorted array.

Note:
You may assume that A has enough space (size 
that is greater or equal to m + n) to hold additional 
elements from B. The number of elements initialized 
in A and B are m and n respectively.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/merge_sorted_array_easy/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/merge_sorted_array_easy/Solution2.py), [C++](https://github.com/lijunhw/leetcode_practice/blob/master/merge_sorted_array_easy/Solution2.cpp)

One clarification is that the size of A and B may be larger than m and n, respectively. So array catenation like "A+B" mostly doesn't work. 

Solution 1 inserts elements in B into A by calling "list.insert()" method. However, according to ["Python Wiki: Time Complexity"](https://wiki.python.org/moin/TimeComplexity), "list.insert()" is O(N) in running time, so the worst case running time is O(N^2)! This may not be as efficient as it looks. 

Solution 2 rearranges the array elements from the back (pick the largest ones and put it in the back). The running time is O(N). 

Obviously, Solution 2 is better. 
