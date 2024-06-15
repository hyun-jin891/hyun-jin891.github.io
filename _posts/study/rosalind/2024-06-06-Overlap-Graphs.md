---
layout: post
title: Overlap Graphs
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, String Algorithm
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Overlap Graphs
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/grph/)

* Description
  * Make graph with a collection of DNA strings in FASTA format having total length at most 10 kbp.
  * Method for making the graph
    * There is a positive number k
    * EX) One DNA string t & Another DNA string s
      * if suffix of t == prefix of s (length of suffix and prefix is k)
      * t is linked to s (print : FASTA_name of t  FASTA_name of s)
  * k = 3

### My Solution
<br>

~~~python
name = input()
file = open(name, 'r')
FASTA_name = []
seqs = []
flag = True


for line in file.readlines():
    if line[0] == '>':
        if flag == True:
            FASTA_name.append(line[1:-1])
            resultSeq = ""
            flag = False
        else:
            seqs.append(resultSeq)
            FASTA_name.append(line[1:-1])
            resultSeq = ""
    else:
        resultSeq += line[:-1]

seqs.append(resultSeq)

for i in range(len(FASTA_name)):
    suffix = seqs[i][-3] + seqs[i][-2] + seqs[i][-1]

    for j in range(len(FASTA_name)):
        if i == j:
            continue
        else:
            prefix = seqs[j][0:3]
            if suffix == prefix:
                print(FASTA_name[i], FASTA_name[j])
~~~

<br>

* DNA sequence can be stored in input file with x (x>1) lines if its length is big
  * Until current line is FASTA_name, collect the DNA sequence line to resultSeq
* With Brute-force Algorithm, we find all combinations satisfying the condition (suffix == prefix)
