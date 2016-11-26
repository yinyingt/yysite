---
layout: post
title: LeetCode Practice
mathjax: false
related: false
comments: true
---

_Last updated: Feb, 2015_


The list below includes my practice on problems from [Leetcode Online Judge](https://oj.leetcode.com/problemset/algorithms/). Various online resources are of great help in such as process, including Leetcode OJ forums and [this blog](http://blog.csdn.net/linhuanmars/). I mostly write them in Python but will add solutions in C++ or Java later. 


## Array and List

Merge and sort:

* [Merge two sorted lists](./merge_two_sorted_lists.html)
* [Merge sorted array](./merge_sorted_array.html)
* [Merge k sorted lists](./merge_k_sorted_lists.html)
* [Sort list](./sort_list.html)

K-sum problems: 

* [Two sum](./two_sum.html)
* [3 sum](./3_sum.html)
* [3 sum closest](./3_sum_closest.html)
* [4 sum](./4_sum.html)


## Binary Search

When to use binary search: if the object is somewhat "sorted", then it has potential to be applied with binary search. 

* [Search insert position](./search_insert_position.html)
* [Search for a range](./search_for_a_range.html)
* [Sqrt(x)](./sqrt_x.html)
* [Search a 2D matrix](./search_a_2d_matrix.html)
* [Search in rotated sorted array I and II](./search_in_rotated_sorted_array_I_and_II.html)
* [Find minimum in rotated sorted array](./find_minimum_in_rotated_sorted_array.html)
* [Find peak element](./find_peak_element.html)


## Matrix

Many matrix problems here do not involve fancy algorithms and data structures. However, index manipulation can be tedious and error prone. 

* [Spiral matrix I and II](./spiral_matrix_I_and_II.html)
* [Rotate image](./rotate_image.html)
* [Validate sudoku](./valid_sudoku.html)
* [Set matrix zeros](./set_matrix_zeros.html)


## Numerical Implementation

In C++ or Java, one may need to worry about the int type overflow in some of those problem . 

* [Palindrome number](./palindrome_number.html)
* [Reverse integer](./reverse_integer.html)
* [Sqrt(x)](./sqrt_x.html)
* [Pow(x,n)](./pow_x_n.html)
* [Divide two integers](./divide_two_integers.html)
* [Max points on a line](./max_points_on_a_line.html)
* [Add binary](./add_binary.html)
* [Add two numbers](./add_two_numbers.html)
* [Plus one](./plus_one.html)
* [Multiply strings](./multiply_strings.html) (empty)

## Bit Manipulation

* [Single number I and II](./single_number_I_and_II.html)
* [Pow(x,n)](./pow_x_n.html)
* [Divide two integers](./divide_two_integers.html)

## Dynamic Programming

Dynamic programming problems exist in many forms. The first step is to tell if the problem can be solved by dynamic programming paradigm, and then try to find the recurrence relationship. Note that dynamic programming problems are not only solvable by recursive design: they can also be done by iterative programs, which may or may not be more efficient. For the recursive approach, one needs to design the API of the recursive call. Some API design is more elegant and efficient that others.

Prototype problems of using dynamic programming: counting/listing different ways of doing something, tree operations, some program using a stack, etc. A good article on this topic is ["dynamic programming: from novice to advanced"](http://www.topcoder.com/tc?d1=tutorials&d2=dynProg&module=Static). 

* [Climbing stairs](./climbing_stairs.html)
* [Decode ways](./decode_ways.html)
* [Maximum subarray](./maximum_subarray.html)
* [Maximum product subarray](./maximum_product_subarray.html)
* [Best time to buy and sell stock I, II and III](./best_time_to_buy_and_sell_stock_I_II_and_III.html)

## Tree

Basic tree traversal methods include preorder, postorder, inorder and level order. The preorder, postorder and inorder traversal can be realized recursively or interatively (using a stack). Level order traversal is realized iteratively using a queue. Zigzag level order traversal is a variant using stacks. 

* Binary tree [inorder](./binary_tree_inorder_traversal.html), [preorder](./binary_tree_preorder_traversal.html) and [postorder](./binary_tree_postorder_traversal.html) traversal. 
* [Binary tree level order traversal I and II](./binary_tree_level_order_traversal_I_and_II.html).
* [Binary tree zigzag level order traversal](./binary_tree_zigzag_level_order_traversal.html)

Binary tree properties:

* [Maximum depth of binary tree](./maximum_depth_of_binary_tree.html)
* [Minimum depth of binary tree](./minimum_depth_of_binary_tree.html)
* [Balanced binary tree](./balanced_binary_tree.html)
* [Same tree](./same_tree.html)
* [Symmetric tree](./symmetric_tree.html)

Path sum problems:

* [Path sum I and II](./path_sum_I_and_II.html)
* [Sum root to leaf numbers](./sum_root_to_leaf_numbers.html)
* [Binary tree maximum path sum](./binary_tree_maximum_path_sum.html)

Binary search trees. 

* Convert sorted [array](./convert_sorted_array_to_binary_search_tree.html) or [list](./convert_sorted_list_to_binary_search_tree.html) to binary search tree
* [Construct binary tree from preorder and inorder traversal](./construct_binary_tree_from_preorder_and_inorder_traversal.html)
* Construct binary tree from inorder and postorder traversal
* [Validate binary search tree](./validate_binary_search_tree.html)
* [Recover binary search tree](./recover_binary_search_tree.html)
* [Unique binary search tree I and II](./unique_binary_search_tree_I_and_II.html)

## Graph

* [Clone graph](./clone_graph.html)
* [Word ladder I and II]
* [Longest consecutive sequence](./longest_consecutive_sequence.html)
* [Word search](./word_search.html)
* [Surrounded regions](./surrounded_regions.html)




