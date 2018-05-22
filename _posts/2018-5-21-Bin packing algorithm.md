---
---
layout: post
title: Bin Packing Algorithm 笔记
categories: algorithms
description: 学习总结
keywords: bin packing, optimization
---

**Bin packing algorithm**

### Bin packing problem
How to pack different sizes of objects into bins?
1. each bin has constant volume
2. try to use least number of bins to pack all the object

![](/images/posts/bin_packing1.png)

### Algorithms
1. First-fit algorithm
First come first serve.
- quick and easy to perfrom
- does not usually lead to an optimal solution

2. First-fit decreasing algorithm
sort all the object in decreasing order. Pack the largest one first.
- quick and easy to perform
- usually better solution than first-fit
- do not always get an optimal solution

3. Full-bin packing
Try to pack several objects to fill up bins one by one.
- usually get a good solution
- can be difficult to perform, if numbers awkward

### Complexity
In computational complexity theory, it is a combinatorial NP-hard problem.


#### Reference:
[Wiki](https://en.wikipedia.org/wiki/Bin_packing_problem)

[Video](https://www.youtube.com/watch?v=kiMFyTWqLhc)
