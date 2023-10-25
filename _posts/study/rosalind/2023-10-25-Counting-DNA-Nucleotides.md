---
layout: post
title: Counting DNA Nucleotides
description: >
  A problem from rosalind "Bioinformatics Stronghold" category
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Counting DNA Nucleotides
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/dna/)

* Description
  * Given DNA sequence, we should calculate the numbers of each base
* Input example : AGGCCTTAGGCAGT
* Output example : 3 3 5 3 (A C G T)

### Biological Background
* DNA sequences consist of 4 bases (A, C, G, T)

### My Solution
<br>

~~~python
d = {}

seqs = input()

for i in range(len(seqs)):
    symbol = seqs[i]
    if symbol not in d:
        d[symbol] = 1
    else:
        d[symbol] += 1

d = sorted(d.items())

for k, v in d:
    print(v, end=' ')
~~~

<br>

* I use the **dictionary** for counting the numbers of each base
* sorted(d.items()) : sorting the dictionary based on the key
  * sorted(d.items(), reverse = True) : reverse sorting
  * sorted(d.items(), key = lambda x : x[1]) : sorting it based on the value
  * sorted(d.items(), key = lambda x : x[1], reverse = True) : reverse sorting
