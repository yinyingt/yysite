---
layout: post
title: LeetCode - Set Matrix Zeros
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/set-matrix-zeroes/), or as follows: 

<pre>
Given a m x n matrix, if an element is 0, set 
its entire row and column to 0. Do it in place.

Follow up:

* Did you use extra space? A straight forward 
solution using O(mn) space is probably a bad idea.
* A simple improvement uses O(m + n) space, 
but still not the best solution.
* Could you devise a constant space solution?
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/set_matrix_zeros_medium/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/set_matrix_zeros_medium/Solution2.py)

Solution 1 does in-place replacement. It visits each element in the matrix. If it is zero, then mark all non-zero elements in the associated row and column to something non-zero (like "None" in Python) while keeping the zeros unchanged. In the end mark all "None"s to zero. The space complexity is O(1), but the runtime complexity is O(MN(M+N)) in the worst case (while O(MN) in the best case). 

Another solution using no extra space is Solution 2. The idea is from [this post](http://blog.csdn.net/linhuanmars/article/details/24066199). It uses the first row and first column as two axes, indicating the coordinates of zeros in the submatrix (aka, matrix[1:][1:]). After that, mark all elements in the rows and columns that have zero value in the axes. One thing to be careful is that the zero filling of first row and column need to be handled separately. See the code in Solution 2 for details. The run-time complexity is O(MN), which is an improvement compared with Solution 1. 
