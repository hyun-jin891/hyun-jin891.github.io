---
layout: post
title: Rabbits and Recurrence Relations
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Dynamic Programming
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Rabbits and Recurrence Relations
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/fib/)

* Description
  * Given n month, k (the numbers of pairs produced from a single pair), we should calculate the numbers of rabit pairs in nth month
* Input example : 6 2
* Output example : 21

### Biological Background
* Fibonacci made assumption of the population of the rabbits
  * The population begins in the first month with a pair of newborn rabbits.
  * Rabbits reach reproductive age after one month.
  * In any given month, every rabbit of reproductive age mates with another rabbit of reproductive age.
  * Exactly one month after two rabbits mate, they produce one male and one female rabbit.
  * Rabbits never die or stop reproducing.

### My Solution
<br>

~~~python
n, k = map(int, input().split())

dp = [0] * (n + 1)


dp[1] = 1


for i in range(2, len(dp)):
    baby = dp[i - 2] * k
    dp[i] = dp[i - 1] + baby

print(dp[-1])
~~~

<br>

* Use dynamic programming
