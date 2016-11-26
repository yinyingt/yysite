---
layout: post
title: Shuffle An Array Randomly
mathjax: false
related: false
comments: true
---

_Created: Mar 2016_

_Last updated: Mar 2016_


Question: How to shuffle an array randomly so that each element is different from its value in the original array? 

The answer is given below, which is an implementation of [Fisher-Yates shuffle algorithm](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle), quoted as:

```
To shuffle an array a of n elements (indices 0..n-1):

for i from n−1 downto 1 do
     j is random integer such that 0 <= j <= i
          exchange a[j] and a[i]
```

The element-wise difference is guaranteed because for each iteration because for each index j the element to be swapped with will be originally from [a[0], a[j-1]] which are not touched by previous shuffling iterations, or from [a[j+1], a[end]] which are exchanged elements from the previous shuffling iteration. 


<script src="https://gist.github.com/lijunhw/da69fd64b8e544a112ec.js?file=random_shuffle.c"></script>

