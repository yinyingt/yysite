---
layout: post
title: LeetCode - Search A 2D Matrix
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/search-a-2d-matrix/), or as follows: 

<pre>
Write an efficient algorithm that searches for a value 
in an m x n matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the 
last integer of the previous row.

For example,

Consider the following matrix:

[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]

Given target = 3, return true.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/search_a_2d_matrix_medium/Solution1.py)

The algorithm for this problem is straightforward. Since the matrix is already sorted row by row, we can first binary search for the row the element is in, and then binary search for the element in that row. However, some care should be taken when searching for the row. See the code above. 

