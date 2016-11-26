---
layout: post
title: LeetCode - Two Sum
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._


Problem description is [here](https://oj.leetcode.com/problems/two-sum/), or as follows: 

<pre>
Given an array of integers, find two numbers 
such that they add up to a specific target number.

The function twoSum should return indices of the 
two numbers such that they add up to the target, 
where index1 must be less than index2. Please note 
that your returned answers (both index1 and 
index2) are not zero-based.

You may assume that each input would have exactly 
one solution.

Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/two_sum_medium/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/two_sum_medium/Solution2.py)

Solution 1 uses hash table to find the two sum pairs. Hash table generation is O(N) in running time, and look-up is O(N) in running time. So the overall running time complexity is O(N), and the space complexity is O(N) because of the hash table. Extra care is needed when using the hash table because there may be repeated elements in the array. 

Solution 2 approaches the problem with array search. The idea is to sort the array at first, and then use two indices initially at two ends going inward until the target is found. The sorting has O(Nlog(N)) running time (also can be done in-place with quicksort), and array search costs O(N) running time. A new array is generated to incorporate the index information, so the space complexity is O(N). However, I think Solution 2 more concise. 
