---
layout: post
title: LeetCode - Decode Ways
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/decode-ways/), or as follows: 

<pre>
A message containing letters from A-Z is 
being encoded to numbers using the 
following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26

Given an encoded message containing digits, 
determine the total number of ways to decode it.

For example,
Given encoded message "12", it could be 
decoded as "AB" (1 2) or "L" (12).

The number of ways decoding "12" is 2. 
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/decode_ways_medium/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/decode_ways_medium/Solution2.py)

The idea behind Solution 1 is like this. Let s be the input string of numbers, and F(s) be the number of ways to decode the input string. There are several cases to think about: 

* If s begins with "10" or "20", then __F(s)=F(s[2:])__. 
* If it is not the case, then if s begins with "0", __F(s)=0__ since there are no matched encodings. 
* If it is still not the case, then if s begins with "1" or "21"~"26", __F(s)=F(s[1:]) + F(s[2:])__. 
* For everything else, such as s begins with "3" ~"9" or "27"~"29", __F(s)=F(s[1:])__. 

Note that in each function call, copies of substrings are passed. It means the space complexity is not optimal, which is ~ O(N^2). There is also _hidden_ run-time overhead associated with the string copying in Python, which is also O(N^2). It explains while this solution technically works, it does not pass LeetCode test engine, yielding _"Time Limit Exceeded"_ error. 

Solution 2 uses similar idea, but decode the string backward from the end. Another big improvement is that it maintains an array num with num[i] being the number of ways to decode string s[:i+1]. The recurrence relationship between s[i] and s[i-1] and/or s[i-2] is:

* If last two digits are "00", then num[i] is 0
* If last two digits are "01"~"09", then num[i] = num[i-1]
* If last two digits are "10" or "20", then num[i] = num[i-2]
* If last two digits are "30", "40", "50", ..., "90", then num[i] = 0
* If last two digits are >"26" but not "30", "40", "50", ..., "90", then num[i] = num[i-1]
* For everything else, num[i] = num[i-1]+num[i-2]

It can be seen that the cases are more tedious and error-prone for Solution 2. And it is done iteratively rather than sorting to recursive calls.  However, because of the global array num, both of run-time and space complexities are only O(N). Solution 2 passes LeetCode test engine. 

There are other (concise) solutions available in [this discussion thread on LeetCode](https://oj.leetcode.com/discuss/questions/oj/decode-ways). Worth a look. 

