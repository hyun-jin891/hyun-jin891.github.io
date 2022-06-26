---
layout: post
title: Dynamic programming
description: >
  Algorithms
tags: [dynamic programming]
use_math: true
categories:
  - study
  - computer science
---
### Dynamic programming
**calculated results → memory**

### Condition
1. small parts x n → Big problems solving
2. identical small problems solving : repeat

### Method
* Top-down
  * completely calculated one is stored in memory → calling it when it needs to be used as big problem is disassociated small problems

  * mainly use recursion

  * recursion's problem : unnecessary repeating calculation → solving by Top-down
  *ex) fibonacci*

  ![그림1](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/14.png?raw=true){: width="600" height="600"}

* Bottom up (mainly use)
  * solving a small problem and stored in memory → using for solving a next problem
  * mainly use loop

### DP table
calculated result will be stored in DP table

* Top-down (fibonacci)

~~~python
dp = [0] * 1000
def fibonacci(x):
  if x == 1 or x == 2:
    return 1
  if dp[x] != 0:
    return dp[x]
  dp[x] = fibonacci(x - 1) + fibonacci(x - 2)
  return dp[x]
~~~

* Bottom up (fibonacci)

~~~python
dp = [0] * 100
dp[1] = 1
dp[2] = 1

for i in range(3, 100):
  dp[i] = dp[i - 1] + dp[i - 2]
~~~
