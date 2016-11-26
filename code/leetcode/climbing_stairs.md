---
layout: post
title: LeetCode - Climbing Stairs
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/climbing-stairs/), or as follows: 

<pre>
You are climbing a stair case. It takes n 
steps to reach to the top.

Each time you can either climb 1 or 2 steps. 
In how many distinct ways can you climb to 
the top? 
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/climbing_stairs_easy/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/climbing_stairs_easy/Solution2.py)

With the hint of dynamic programming, the solution should be very straight. If F(n) is the number of ways to climb an n-step stair case, the recurrence relationship is 

{% highlight bash %}
F(n) = F(n-1) + F(n-2)
{% endhighlight %}

So it is just like Fibonacci sequence except for F(0)=0, F(1)=1, and F(2)=2. We can solve the problem with recursive calls like Solution 1, or just directly calculate F(n) iteratively in Solution 2 (well, Solution 2 is better in this case since it only uses O(1) memory). 
