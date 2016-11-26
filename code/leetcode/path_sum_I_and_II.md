---
layout: post
title: LeetCode - Path Sum (I and II)
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/path-sum/), or as follows: 

<pre>
Given a binary tree and a sum, determine if the 
tree has a root-to-leaf path such that adding up 
all the values along the path equals the given sum.

For example:
Given the below binary tree and sum = 22,

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

return true, as there exist a root-to-leaf 
path 5->4->11->2 which sum is 22.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/path_sum_easy/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/path_sum_easy/Solution2.py)

An (enhanced) variant of this problem is [here](https://oj.leetcode.com/problems/path-sum-ii/), or as follows: 

<pre>
Given a binary tree and a sum, find all root-to-leaf 
paths where each path's sum equals the given sum.

For example:
Given the below binary tree and sum = 22,

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1

return

[
   [5,4,11,2],
   [5,8,4,5]
]
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/path_sum_II_medium/Solution1.py)

