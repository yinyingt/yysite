---
layout: post
title: LeetCode - Unique Binary Search Tree (I and II)
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._


Problem description is [here](https://oj.leetcode.com/problems/unique-binary-search-trees/), or as follows: 

<pre>
Given n, how many structurally unique BST's (binary 
search trees) that store values 1...n?

For example,
Given n = 3, there are a total of 5 unique BST's.

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/unique_binary_search_tree_medium/Solution1.py)

A follow-up on this problem is [here](https://oj.leetcode.com/problems/unique-binary-search-trees-ii/), that is, to generate all unique BSTs. 

<pre>
Given n, generate all structurally unique BST's (binary 
search trees) that store values 1...n.

For example,
Given n = 3, your program should return all 5 unique BST's shown below.

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/unique_binary_search_tree_II_medium/Solution1.py)
