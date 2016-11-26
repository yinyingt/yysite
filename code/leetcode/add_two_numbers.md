---
layout: post
title: LeetCode - Add Two Numbers
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/add-two-numbers/), or as follows: 

<pre>
You are given two linked lists representing two 
non-negative numbers. The digits are stored in 
reverse order and each of their nodes contain a 
single digit. Add the two numbers and return it 
as a linked list.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/add_two_numbers_medium/Solution1.py)

This solution is intuitive since it resembles a normal addition operation of two numbers. A subroutine adding two numbers is used to handle the carrier. One thing to note is that if the one number is longer than the other, don't just append the remaining linked list to the result. Addition should continue being carried out on the remaining linked list because there may be non-zero carrier from the previous part. 
