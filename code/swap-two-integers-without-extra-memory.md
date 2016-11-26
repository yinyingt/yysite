---
layout: post
title: Swap Two Integers Without Extra Memory
mathjax: false
related: false
comments: true
---

__Question: How to swap two integers without using extra memory.__ 

This is a question I got asked in an interview for an embedded engineer position. There are two ways I can think of as follows:

Solution 1: 

```
void swap(int* a, int* b) {
    *a = *a + *b;
    *b = *a - *b;  // b becomes a
    *a = *a - *b;  // a becomes b
}
```

Solution 2:

```
void swap(int* a, int* b) {
    *a = (*a)^(*b);
    *b = (*a)^(*b);  // b becomes a
    *a = (*a)^(*b);  // a becomes b
}
```

So all is about bit manipulation. This code is simple, but can be useful for some embedded systems which do not have tons of memory. 
