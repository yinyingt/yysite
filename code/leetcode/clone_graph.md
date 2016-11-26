---
layout: post
title: LeetCode - Clone Graph
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._

Problem description is [here](https://oj.leetcode.com/problems/clone-graph/), or as follows: 

<pre>
Clone an undirected graph. Each node in the graph 
contains a label and a list of its neighbors.

OJ's undirected graph serialization:

Nodes are labeled uniquely.
We use # as a separator for each node, and , as a 
separator for node label and each neighbor of the node.

As an example, consider the serialized graph 
{0,1,2#1,2#2,2}.

The graph has a total of three nodes, and therefore 
contains three parts as separated by #.

* First node is labeled as 0. Connect node 0 to both nodes 1 and 2.
* Second node is labeled as 1. Connect node 1 to node 2.
* Third node is labeled as 2. Connect node 2 to node 2 (itself), thus forming a self-cycle.

Visually, the graph looks like the following:

       1
      / \
     /   \
    0 --- 2
         / \
         \_/
</pre>


* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/clone_graph_medium/Solution1.py)
* Solution 2: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/clone_graph_medium/Solution2.py)

