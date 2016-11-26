---
layout: post
title: LeetCode - Palindrome Number
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/palindrome-number/), or as follows: 

<pre>
Determine whether an integer is a palindrome. 
Do this without extra space.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/palindrome_number_easy/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/palindrome_number_easy/Solution2.py)

First of all, according to [Wikipedia's definition](https://en.wikipedia.org/wiki/Palindrome), "a palindrome is a word, phrase, number, or other sequence of characters which reads the same backward or forward". 

In Solution 1, the length of the integer is first determined, and then the comparison of digits starts from two ends and progresses toward the middle. This solution uses division operator "/" and modulo operator "%" extensively. The solution uses constant space, and the run-time complexity is O(N). 

Solution 2 uses the same idea but implements the code a little differently (it manipulates the divisor rather than the number itself in Solution 1). 

Although the algorithm is somewhat mundane, this type of problem needs practice. 
