---
layout: post
title: LeetCode - Valid Sudoku
mathjax: false
related: false
comments: true
---

_Click [here](./index.html) to go back to LeetCode summary page._


Problem description is [here](https://oj.leetcode.com/problems/valid-sudoku/), or as follows: 

<pre>
Determine if a Sudoku is valid. The Sudoku board could 
be partially filled, where empty cells are filled with 
the character '.'.

Note:
A valid Sudoku board (partially filled) is not necessarily 
solvable. Only the filled cells need to be validated. 
</pre>

* Solution 1: [Python](https://github.com/lijunhw/leetcode_practice/blob/master/valid_sudoku_easy/Solution1.py)

Note that this problem only asks whether a sudoku map is valid, not whether it is solvable. A valid sudoku means the initial number arrangement does not violate the sudoku rules. It may or may not be solvable.
 
The algorithm is all brute-force based on sudoku rules: to validate rows, columns, and 3x3 sections. Nothing fancy here. 


