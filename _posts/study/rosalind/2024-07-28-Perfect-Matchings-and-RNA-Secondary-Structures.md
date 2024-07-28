---
layout: post
title: Perfect Matchings and RNA Secondary Structures
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Combinatorics, String Algorithm
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Perfect Matchings and RNA Secondary Structures
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/pmch/)

* Description
  * Matching: "a collection of edges that no node belongs to more than one edge in the collection"
    * == one base can form only one base pair
  * Perfect Matching: "If graph contains an even number of nodes (say 2n ), then a matching on the graph is perfect if it contains n edges, which is clearly the maximum possible"
    * == all bases have to participate in base pairing
  * We have to calculate total possible number of perfect matchings of basepair edges in the bonding graph of s

### My Solution
* Sketch
  * Use "Combinatorics"
    * If AU = x & GC = y, result is (x/2)! * (y/2)!

* Code

<br>

~~~python
from math import factorial

def AU_count(seq):
    res = 0

    for i in range(len(seq)):
        if seq[i] == 'A' or seq[i] == 'U':
            res += 1

    return res


name = input()
f = open(name, 'r')

f.readline()
rnaSeq = ""

for line in f.readlines():
    rnaSeq += line[:-1]

AU_pair = AU_count(rnaSeq)
GC_pair = len(rnaSeq) - AU_pair


print(factorial(AU_pair//2) * factorial(GC_pair//2))

f.close()
~~~
