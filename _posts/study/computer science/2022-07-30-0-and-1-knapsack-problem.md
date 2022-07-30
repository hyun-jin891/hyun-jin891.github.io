---
layout: post
title: 0 and 1 Knapsack problem
description: >
  DP
tags: [kanpsack problem]
use_math: true
categories:
  - study
  - computer science
---
## Knapsack problem

* Condition
  * There is a bag (allowing maximum weight W<sub>max</sub>)
  * There are various things that will be added to the bag (They have specific value V and specific weight W)
* **Question**
  What is the maximum value (V<sub>max</sub> , sum of things added to the bag) not violating the maximum weight?
* Type
  1. Fractional Knapsack<br>
  2. 0-1 Knapsack<br>

## 0-1 Knapsack Problem
* The thing cannot be divided (One thing that will be added to the bag should be whole thing)
* can be solved by DP

## DP table
![그림1](/assets/img/72.PNG?raw=true){: width="600" height="600"}<br>

* Let specific location of row == i
* Let specific location of column == j
* dp[i][j] : V<sub>max</sub> (combination of Thing 1 ~ Thing i not violating the maximum weight j)

## Recurrence relation
* It is important to check whether a specific thing (specific row) can be added to the bag or not
* should check the possibility that a specific thing can be added to the bag
  * if this thing's weight > W<sub>max</sub>
    * Unconditionally impossible
    * dp[i][j] = dp[i - 1][j]
  * else
    * possible
    * In this case, we should compare between maximum value of the thing being included and that of the thing not being
    * dp[i - 1][j - thing's weight] + thing(unconditionally enter into the bag)'s value (former)
    * dp[i - 1][j] (latter)
    * dp[i][j] = max(dp[i - 1][j - thing's weight] + thing's value, dp[i - 1][j])

## Well explanation
[blog](https://gsmesie692.tistory.com/113)
