---
layout: post
title: Mortal Fibonacci Rabbits
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Dynamic Programming
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Mortal Fibonacci Rabbits
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/fibd/)

* Description
  * Given n month and m month : n is current month, m is life cycle of a rabbit
  * Reproduction : Born in n<sup>th</sup> month → First reproduction happens in (n+2)<sup>th</sup> month
    * Reproduction is possible even when it dies
    * That is, +1 and -1 happen simultaneously in one month
  * Die : ex) m = 3 → One rabbit which is born in n<sup>th</sup> month dies in (n + 3)<sup>th</sup> month
* Input example : 6 3
* Output example : 4

### My Solution
<br>

~~~python
n, m = map(int, input().split())

newRabbit = [0] * (n + 1)
newRabbit[1] = 1
totalRabbit = [0] * (n + 1)
totalRabbit[1] = 1

for i in range(2, n + 1):
    newRabbit[i] = totalRabbit[i - 1] - newRabbit[i - 1]    
    totalRabbit[i] = totalRabbit[i - 1] - newRabbit[i - m] + newRabbit[i]

print(totalRabbit[-1])
~~~

<br>

* Use dynamic programming
* Prepare two dp table : number of new rabbits in i<sup>th</sup> month & total number of rabbits in i<sup>th</sup> month
* 1 means 1 pair (2 rabbits)
* In 1<sup>st</sup>, one pair of rabbits is born
* newRabbit[i] = totalRabbit[i - 1] - newRabbit[i - 1]
  * In (i - 1)<sup>th</sup> month, only newRabbit cannot reproduce in next month
* totalRabbit[i] = totalRabbit[i - 1] - newRabbit[i - m] + newRabbit[i]
  * Total number of rabbits in i<sup>th</sup> month = total number of rabbits in (i - 1)<sup>th</sup> month - number of rabbits which die + newRabbit in i<sup>th</sup> month
