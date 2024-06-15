---
layout: post
title: Mendel's First Law
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Probability Calculation
tags: [rosalind problem]

categories:
  - study
  - rosalind
---

### Mendel's First Law
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/iprb/)

* Description
  * Given three integers (k : homozygous dominant, m : heterozygous, n : homozygous recessive), we need to calculate the probability that next generation has dominant allele from random mate
* Input example

<br>

~~~
2 2 2
~~~

<br>

* Output example

<br>

~~~
0.78333
~~~

### My Solution
<br>

~~~python
def probability(k, m, n):
    total = k + m + n
    pair1 = (k / total) * ((k - 1) / (total - 1))
    pair2 = (k / total) * (m / (total - 1))
    pair3 = (k / total) * (n / (total - 1))
    pair4 = (m / total) * ((m - 1) / (total - 1)) * 0.75
    pair5 = (m / total) * (n / (total - 1)) * 0.5
    pair6 = (m / total) * (k / (total - 1))
    pair7 = (n / total) * (k / (total - 1))
    pair8 = (n / total) * (m / (total - 1)) * 0.5

    return pair1 + pair2 + pair3 + pair4 + pair5 + pair6 + pair7 + pair8


file_name = input()
f = open(file_name, 'r')

line = f.readline()

k, m, n = map(int, line.split())

print(probability(k, m, n))
~~~

<br>

* we need to consider Sampling without replacement
