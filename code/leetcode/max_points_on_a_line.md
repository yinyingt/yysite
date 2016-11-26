---
layout: post
title: LeetCode - Max Points on A Line
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/max-points-on-a-line/), or as follows: 

<pre>
Given n points on a 2D plane, find the maximum 
number of points that lie on the same straight line.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/max_points_on_a_line_hard/Solution1.py)

A brute-force way is iterate over all possibile combinations which results in O(N^3) running time and O(1) space. 

Solution here is more computationally efficient using hash tables. The idea is to calculate slopes of all other points w.r.t. a point. If the slopes equals, they are on the same line. A hash table is used to stat the points falling on the same line in an iteration (key is slope; value is # of pairs with the same slope). And then we iterate over the rest, excluding the points already been iterated. This solution results in O(N^2) running time and O(N) space (for hash table).

The subtlety needs handling here is when there are duplicated points in the set. See the code on how to handle that. 
