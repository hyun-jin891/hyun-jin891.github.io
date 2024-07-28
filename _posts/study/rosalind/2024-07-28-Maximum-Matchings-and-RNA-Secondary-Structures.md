---
layout: post
title: Maximum Matchings and RNA Secondary Structures
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Dictionary, String Algorithm, Combinatorics
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Maximum Matchings and RNA Secondary Structures
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/mmch/)

* Description
  * Matching: "a collection of edges that no node belongs to more than one edge in the collection"
    * == one base can form only one base pair
  * Perfect Matching: "If graph contains an even number of nodes (say 2n ), then a matching on the graph is perfect if it contains n edges, which is clearly the maximum possible"
    * == all bases have to participate in base pairing
  * **Perfect Matching X**
    * Some bases do not participate in base pairing
    * count(A) != count(T) & count(G) != count(C)
    * We have to calculate the number of maximum matchings
      * Maximum matching: "forming as many base pairs as possible in an RNA string"

### My Solution
* Sketch
  * Use "Combinatorics"
    * <sub>max(count_A, count_T)</sub>P<sub>min(count_A, count_T)</sub> x <sub>max(count_G, count_C)</sub>P<sub>min(count_G, count_C)</sub>

* Code

<br>

~~~python
from math import comb
from math import factorial

base_dict = {'A' : 0, 'U' : 0, 'G' : 0, 'C' : 0}

seq = ""

name = input()
f = open(name, 'r')
f.readline()

for line in f.readlines():
    seq += line[:-1]

for i in range(len(seq)):
    base_dict[seq[i]] += 1

res = 1

count_A = base_dict['A']
count_U = base_dict['U']
count_G = base_dict['G']
count_C = base_dict['C']

if count_A >= count_U:
    res *= comb(count_A, count_U) * factorial(count_U)
else:
    res *= comb(count_U, count_A) * factorial(count_A)

if count_G >= count_C:
    res *= comb(count_G, count_C) * factorial(count_C)
else:
    res *= comb(count_C, count_G) * factorial(count_G)

print(res)

f.close()
~~~
