---
layout: post
title: LeetCode - Binary Tree ZigZag Level Order Traversal
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/binary-tree-zigzag-level-order-traversal/), or as follows: 

<pre>
Given a binary tree, return the zigzag level order traversal of its 
nodes' values. (ie, from left to right, then right to left for the 
next level and alternate between).

For example:
Given binary tree {3,9,20,#,#,15,7},

    3
   / \
  9  20
    /  \
   15   7

return its zigzag level order traversal as:

[
  [3],
  [20,9],
  [15,7]
]
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/binary_tree_zigzag_level_order_traversal_medium/Solution1.py)


It is still breadth first search, but with two stacks. We can still reuse the idea in ["Binary Tree Level Order Traversal"](./binary_tree_level_order_traversal_I_and_II.html) problem except that we use two stacks and a level number (root level is 0). If level number is even, print out results from left to right. If odd, print out in reverse order. This is achieved by pushing node to a helper stack but popping node from main stack. The traversal order repeats every other level because of the two stacks. Some discussion on the recursive implementation of this one is [here](https://oj.leetcode.com/discuss/oj/binary-tree-zigzag-level-order-traversal). See the code for details. 
