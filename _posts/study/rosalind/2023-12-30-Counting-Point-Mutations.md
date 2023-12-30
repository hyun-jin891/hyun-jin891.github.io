---
layout: post
title: Counting Point Mutations
description: >
  A problem from rosalind "Bioinformatics Stronghold" category
tags: [rosalind problem]

categories:
  - study
  - rosalind
---

### Counting Point Mutations
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/hamm/)

* Description
  * Given two sequences, we need to calculate the hamming distance
* Input example

<br>

~~~
GAGCCTACTAACGGGAT
CATCGTAATGACGGCCT
~~~

<br>

* Output example

<br>

~~~
7
~~~

### Biological Background
* Hamming Distance
  * The number of elements that are different in two sequences
* "We are interested in minimizing the number of (point) mutations separating two species because of the biological principle of **parsimony**, which demands that evolutionary histories should be as simply explained as possible." - Rosalind (Counting Point Mutations)
* **Parsimony Principle** : When we make phylogenetic tree of life, we assume evolution will happen in direction to minimum change
  * If there are lots of possible trees, we gonna pick up one that went through minimum mutations
  * Consequently, it is important to count the mutations
  * [Parsimony Principle](https://m.blog.naver.com/wnsrl5/221406314544)

### My Solution
<br>

~~~python
file_name = input()

f = open(file_name, 'r')

seq1 = f.readline()
seq2 = f.readline()

count = 0

for i in range(len(seq1)):
    if seq1[i] != seq2[i]:
        count += 1

print(count)
~~~

<br>
