---
layout: post
title: LeetCode - Sort List
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/sort-list/), or as follows: 

<pre>
Sort a linked list in O(nlogn) time using constant space 
complexity.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/sort_list_medium/Solution1.py), [C++](https://github.com/lijunhw/leetcode_practice/blob/master/sort_list_medium/Solution1.cpp)

It is a typical [merge sort](http://en.wikipedia.org/wiki/Merge_sort) with top-down implementation. It is essentially a divide-and-conquer method: in each sub-probelm, divide the list in half to two **sorted** sub-lists, and merge them into a single sorted list. In the end, one will come to a situation where there are only two or one nodes in the list, meaning, respectively, each sub-list only has one node or is just empty (none). Therefore the base cases are sorted as is. The merge subroutine "merge2lists" is borrowed from the solution in ["Merge Two Sorted Lists"](./merge_two_sorted_lists.html) problem, in which the merge can be done with constant space for linked lists. For arrays, merge sort has space complexity of O(N). 

Another popular sorting algorithm with average running time complexity of O(Nlog(N)) is quick sort. The algorithm description and a C implementation can be found [here](https://github.com/lijunhw/codedrop/blob/master/quicksort/quicksort.c). It can do in-place sort for arrays. However, the worst running time can up to O(N^2) if the initial array is not randomly shuffled (almost sorted). 

