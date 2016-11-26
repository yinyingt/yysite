---
layout: post
title: LeetCode - 4 Sum
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/4sum/), or as follows: 

<pre>
Given an array S of n integers, are there elements a, 
b, c, and d in S such that a + b + c + d = target? 
Find all unique quadruplets in the array which gives 
the sum of target.

Note:
* Elements in a quadruplet (a,b,c,d) must be in 
non-descending order. (ie, a ≤ b ≤ c ≤ d)
* The solution set must not contain duplicate quadruplets.

* For example, given array S = {1 0 -1 0 -2 2}, 
and target = 0.

    A solution set is:
    (-1,  0, 0, 1)
    (-2, -1, 1, 2)
    (-2,  0, 0, 2)
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/4_sum_medium/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/4_sum_medium/Solution2.py)

Solution 1 uses the solution in ["3 Sum"](./3_sum.html) problem as a subroutine, thus collapses the problem to a three-sum problem and further to two-sum problem. The running time complexity is O(N^3) because of the three for loops. This scheme works for any k-sum problem, resulting running time complexity of O(N^k). 

Solution 2 is smarter, optimized for this four-sum problem. The idea is to do two "two-sum" search. First, generate a hash table with all sum combinations of two elements in the array as the keys, and corresponding index combinations as the values. Convert the hash table to an sorted array after that. Second, do an array search on the sorted array like the solutions in ["Two Sum"](./two_sum.html) problem. Running time complexities: hash table generation is O(N^2), sorting is O(N^2log(N)) (since the hash table size is O(N^2)), and two-sum search in the second step is O(N^2). So the overall running time complexity is O(N^2log(N)). Space complexity is O(N^2) because of the hash table. 

