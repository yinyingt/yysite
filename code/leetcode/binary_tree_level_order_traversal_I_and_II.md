---
layout: post
title: LeetCode - Binary Tree Level Order Traversal (I and II)
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/binary-tree-level-order-traversal/), or as follows: 

<pre>
Given a binary tree, return the level order traversal of its 
nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree {3,9,20,#,#,15,7},

    3
   / \
  9  20
    /  \
   15   7

return its level order traversal as:

[
  [3],
  [9,20],
  [15,7]
]
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/binary_tree_level_order_traversal_easy/Solution1.py)

The definition of level travesal is already explained in the problem description (more like a breadth first search). A queue is utilized here, therefore recursive implementation is not possible (recursive calls use stacks implicitly). The output format of this problem is different from previous [inorder](./binary_tree_inorder_traversal.html)/[preorder](./binary_tree_preorder_traversal.html)/[postorder](./binary_tree_postorder_traversal.html) traversal problems, thus extra code is needed. 

A variant of this problem is [here](https://oj.leetcode.com/problems/binary-tree-level-order-traversal-ii/), or as follows:

<pre>
Given a binary tree, return the bottom-up level order traversal of 
its nodes' values (i.e., from left to right, level by level from leaf 
to root).

For example:
Given binary tree {3,9,20,#,#,15,7},

    3
   / \
  9  20
    /  \
   15   7

return its bottom-up level order traversal as:

[
  [15,7],
  [9,20],
  [3]
]

</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/binary_tree_level_order_traversal_II_easy/Solution1.py)

One solution is use most part of the previous binary tree level traversal but insert the level list at the head of the results rather than append it so that an reversal is not needed. There is discussion on the LeetCode forum [here](https://oj.leetcode.com/discuss/5353/there-better-regular-level-order-traversal-reverse-result). This problem is somewhat redundant. 
