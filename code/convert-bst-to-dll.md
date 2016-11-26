---
layout: post
title: Convert Binary Search Tree to Doubly Linked List
mathjax: false
related: false
comments: true
---

_Created: Mar 2016_

_Last updated: Mar 2016_



__Question: Convert a binary search tree to a sorted double-linked list. We can only change the target of pointers, but cannot create any new nodes. In the resulting doubly linked tree, the left pointer points to the node with smaller value, while the right pointer points to the node with larger value. For example,__

![BST to DLL example](http://1.bp.blogspot.com/-CHLEHGKaDVU/TmlNi606VXI/AAAAAAAAA9Q/NbLxWFIk4Wg/s1600/01_Figure1.PNG)

_The picture above is stolen from [here](http://codercareer.blogspot.com/2011/09/interview-question-no-1-binary-search.html)_


This is a typical tree question I once got asked in an interview, which is actually actually documented [here](http://codercareer.blogspot.com/2011/09/interview-question-no-1-binary-search.html) and lots of other places. 

The first impression is that one can solve this problem with dynamic programming (such as recursive in-order traversal). However, handling the left and right sub-trees will be different because the node manipulation between the current node and the sub-tree is : 


* For left sub-tree: between the current root node and the tail node of the left sub-tree
* For right sub-tree: between the current root node and the head of right sub-tree. 

This picture says it all (the picture is also stolen from [here](http://codercareer.blogspot.com/2011/09/interview-question-no-1-binary-search.html)): 

![BST to DLL analysis](http://1.bp.blogspot.com/-0uwiSKPjT0w/TmlN8JKR_zI/AAAAAAAAA9U/1JFbTQ0J80Q/s1600/01_Figure2.PNG)


Therefore it is necessary to keep track of the last node of sub-tree. Two ways: 

* (Dumb) Iterate the whole sub-tree to find its tail 
* (More efficient) Keep a record of the tail node 

Obviously, the latter is a better idea. 

My solution in Python is as follows (the implementation is a little different from [this post](http://codercareer.blogspot.com/2011/09/interview-question-no-1-binary-search.html)): 

<script src="https://gist.github.com/lijunhw/93acd33914b3053f6c52c8c63421e754.js?file=bst2dll.py"></script>