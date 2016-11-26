---
layout: post
title: LeetCode - Add Binary
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/add-binary/), or as follows: 

<pre>
Given two binary strings, return their sum 
(also a binary string).

For example,
a = "11"
b = "1"
Return "100". 
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/add_binary_easy/Solution1.py)


In Solution 1, the two strings are added bit by bit, and the final result is returned using bin() method in Python which converts an integer to a binary number string. The only thing to note is that the carrier should be taken care of on each addition. 

