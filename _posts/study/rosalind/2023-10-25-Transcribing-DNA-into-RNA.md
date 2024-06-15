---
layout: post
title: Transcribing DNA into RNA
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, String Algorithm
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Transcribing DNA into RNA
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/rna/)

* Description
  * Given DNA coding sequence, we should deduce the sequence of mRNA produced from this DNA sequence
* Input example : AGGCCTTAGGCAGT
* Output example : AGGCCUUAGGCAGU

### Biological Background
* DNA sequences consist of 4 bases (A, C, G, T)
* RNA sequences consist of 4 bases (A, C, G, U)
* DNA consists of 2 strands
  * coding sequence : has same sequences with mRNA having information except T (should be changed to U)
  * noncoding sequence : complement version of coding sequence (A ↔ T, G ↔ C)

### My Solution
<br>

~~~python
DNA_seq = input()
RNA_seq = ""

for i in range(len(DNA_seq)):
    symbol = DNA_seq[i]

    if symbol == 'T':
        RNA_seq += 'U'
    else:
        RNA_seq += symbol

print(RNA_seq)
~~~

<br>

* just change T to U
