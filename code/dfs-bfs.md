---
layout: post
title: DFS and BFS
mathjax: false
related: false
comments: true
---

_Created: Mar 2016_

_Last updated: Mar 2016_


* What is the difference between DFS and BFS? 

Generally, DFS is better when searching node deep in the branch, while BFS is better when the node is shallow, which is self-explanatory from the names. 

Implementation-wise, DFS and BFS is essentially the same except that the former uses a stack while the latter uses a queue. 

This kind of discussion is readily available online, such as [this one](https://www.quora.com/Graph-Theory-What-is-the-difference-between-depth-first-search-and-breadth-first-search). 

The implementation of both in Python is as follows. Simple but powerful idea. 

<script src="https://gist.github.com/lijunhw/581916e384fbd5736d734948a1b0281e.js?file=dfs_bfs_test.py"></script>

