---
layout: post
title: LeetCode - Merge Two Sorted Lists
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/merge-two-sorted-lists/), or as follows: 

<pre>
Merge two sorted linked lists and return it as a new list. 
The new list should be made by splicing together the nodes of 
the first two lists.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/merge_two_sorted_lists_easy/Solution1.py), [C++](https://github.com/lijunhw/leetcode_practice/blob/master/merge_two_sorted_lists_easy/Solution1.cpp)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/merge_two_sorted_lists_easy/Solution2.py), [C++](https://github.com/lijunhw/leetcode_practice/blob/master/merge_two_sorted_lists_easy/Solution2.cpp)

Both solutions work with space complexity of O(1). Solution 1 uses a pointer and grows the resulting list by picking the current minimum value node in List 1 and List 2. Solution 2 merges List 2 into List 1 but inserting the smaller value node before the node in List 1. Both solutions are easy to understand, although Solution 1 appears to be more intuitive in my personal opinion. 
 
This solution can be used as a subroutine in other problems, such as the ["Sort List"](./sort_list.html) problem. 

