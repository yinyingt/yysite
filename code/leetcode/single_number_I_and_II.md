---
layout: post
title: LeetCode - Single Number (I and II)
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/single-number/), or as follows: 

<pre>
Given an array of integers, every element appears 
twice except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. 
Could you implement it without using extra memory? 
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/single_number_medium/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/single_number_medium/Solution2.py)

This is a typical bit manipulation problem, exploring the properties of exclusive-or (XOR) operation. We know that _B xor B_ is 0, so _A xor B xor B_ results in A. Therefore, the result of XORing all elements in that array equals the single number. 

Solution 1 and 2 are essentially the same except for the different syntax form. Both have linear runtime without using extra memory. 

Another problem using the properties of XOR is ["Swap Two Integers Without Extra Memory"](../swap_two_integers_without_extra_memory.html). 

A variant problem is [here](https://oj.leetcode.com/problems/single-number-ii/), or as follows: 

<pre>
Given an array of integers, every element appears 
three times except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. 
Could you implement it without using extra memory? 
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/single_number_II_medium/Solution1.py)

Solution 1 has linear runtime, and O(1) memory usage. The algorithm can be generalized to solve an array with elements appearing k times except for one. 

The idea is to convert an int to a 32-element array with each element representing the bit in its corresponding binary format. For example, 7 corresponds to S = [0, 0, ...., 0, 1,1,1]. Now add all elements in the input array in form of this representation, and giving the final sum S. The remainder of modulo 3 operation on each element of S corresponds to the single number. 

See the code for details. 

