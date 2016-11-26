---
layout: post
title: LeetCode - Best Time to Buy and Sell Stock (I, II and III)
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem I description is [here](https://oj.leetcode.com/problems/best-time-to-buy-and-sell-stock/), or as follows: 

<pre>
Say you have an array for which the ith element 
is the price of a given stock on day i.

If you were only permitted to complete at most 
one transaction (ie, buy one and sell one share 
of the stock), design an algorithm to find the 
maximum profit.
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/best_time_to_buy_and_sell_stock_easy/Solution1.py)


This problem needs more clarification as discussed [here](https://oj.leetcode.com/discuss/1638/need-more-explanation-about-the-problem). One can only buy once and sell once in the whole series. One has to buy first and then sell, aka cannot short stocks. For example, [8, 2, 4, 6, 3, 5] yields max profit of -2+6=4. Of course this is more of a simulated situation because in reality one has to make decision today by only having the previous data. The purchase today will further affect the consequent transactions in the future which is yet known today. But now we have all the history data already and try to figure out if we buy in that day and sell in another day we can make max profit, which is nonsense in reality. But it is fair enough for a small problem. 

The solution again uses the local-global strategy as in ["Maximum Subarray"](./maximum_subarray.html) problem. Since we want to buy low and sell high, we maintain a local maximum profit, with "local" meaning selling the stock today. We further keep a variable of minimum price till yesterday, so the local maximum profit is price today minus minimum price till yesterday. In the end update the global max profit if necessary. The code is pretty straightforward. 

A modified version (Problem II) is [here](https://oj.leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/), or as follows:

<pre>
Say you have an array for which the ith element 
is the price of a given stock on day i.

Design an algorithm to find the maximum profit. 
You may complete as many transactions as you like 
(ie, buy one and sell one share of the stock multiple 
times). However, you may not engage in multiple 
transactions at the same time (ie, you must sell 
the stock before you buy again).
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/best_time_to_buy_and_sell_stock_II_medium/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/best_time_to_buy_and_sell_stock_II_medium/Solution2.py)

Since we can do multiple transactions, we can detect the rising trend subsections of the price, and sum up the max profits of those subsections. What if the price is always going down? We simply don't do transaction, which is already part of the algorithm. This is described in Solution 1. 

Solution 2 is even easier: sum up all one-day local profits (i.e., if making profits by buying yesterday and selling today) and we have max profit globally. 

Both have run-time complexity of O(N) and space complexity of O(1), but Solution 2 is much more concise and thus better for this problem. 

A further modified version (Problem III) is [here](https://oj.leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/), or as follows:

<pre>
Say you have an array for which the ith element 
is the price of a given stock on day i.

Design an algorithm to find the maximum profit. 
You may complete at most two transactions.

Note:
You may not engage in multiple transactions at 
the same time (ie, you must sell the stock before 
you buy again).
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/best_time_to_buy_and_sell_stock_III_hard/Solution1.py)

Note that question here becomes you can do __at most two transactions__. The idea in Solution 1 is to break the problem to two subproblems: for a given index i, find the global max profit for left subarray and right subarray respectively, given that only at most one transaction is allow for each subarray. Since there are at most two transactions in total, the max profit is the sum of those two after sweep i from 0 to N-1 with N being the length of array. It is easy to see that time complexity is O(N) and space complexity is O(N). This algorithm is discussed [here](https://oj.leetcode.com/discuss/14806/solution-sharing-commented-code-o-n-time-and-o-n-space). 

However, there are solutions with time complexity of O(N) and space complexity of O(1), discussed on LeetCode [here](https://oj.leetcode.com/discuss/18330/is-it-best-solution-with-o-n-o-1). Besides, this problem can also be generalized to k transaction times as discussed [here](http://blog.csdn.net/linhuanmars/article/details/23236995). 
