---
layout: post
title: Prefix Sum
description: >
  Algorithm
tags: [prefix sum]
use_math: true
categories:
  - study
  - computer science
---
### Prefix Sum
* 구간 합
* for solving the sum of elements in specific index range of array
  * Original method (O(n) considering the part only that solves the sum, inefficient)<br>
  ~~~python
  l = [1, 3, 4, 8, 6, 2] # sum of l[1 : 4] == 15
  result = 0

  for i in range(1, 4):
    result += l[i]

  print(result)
  ~~~

  * Prefix Sum (O(1) considering the part only that solves the sum, efficient)<br>
  ~~~python
  l = [1, 3, 4, 8, 6, 2]
  result = [0] * 6
  result[0] = 1

  for i in range(1, len(l)):
    result[i] = result[i - 1] + l[i]

  print(result[3] - result[0])    # O(1)
  ~~~
