---
layout: post
title: Complementing a Strand of DNA
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, String Algorithm
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Complementing a Strand of DNA
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/revc/)

* Description
  * Given DNA sequence, we should deduce the reverse complement
* Input example : AGGCCTTAGGCAGT
* Output example : ACTGCCTAAGGCCT

### Biological Background
* DNA consists of 2 strands
  * coding sequence : has same sequences with mRNA having information except T (should be changed to U)
  * noncoding sequence : complement version of coding sequence (A ↔ T, G ↔ C)
* 2 strands are antiparellel
  * 5'-AGGCT-3'
  * 3'-TCCGA-5'
  * **Reverse complement** of 5'-AGGCT-3' is 5'-AGCCT-3'

### My Solution
<br>

~~~python
DNA_seq = input()
reverse_c = ""

for i in range(len(DNA_seq) - 1, -1, -1):
    symbol = DNA_seq[i]
    newSymbol = ''

    if symbol == 'A':
        newSymbol = 'T'
    elif symbol == 'T':
        newSymbol = 'A'
    elif symbol == 'G':
        newSymbol = 'C'
    elif symbol == 'C':
        newSymbol = 'G'
    reverse_c += newSymbol

print(reverse_c)
~~~

<br>

* Search by the reverse direction on suggested DNA sequence
* add the complement symbol into reverse_c
