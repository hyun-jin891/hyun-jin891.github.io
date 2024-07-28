---
layout: post
title: Catalan Numbers and RNA Secondary Structures
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Dictionary, Recursion, String Algorithm, Combinatorics, Dynamic Programming
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Catalan Numbers and RNA Secondary Structures
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/cat/)

* Description
  * Matching: "a collection of edges that no node belongs to more than one edge in the collection"
    * == one base can form only one base pair
  * Perfect Matching: "If graph contains an even number of nodes (say 2n ), then a matching on the graph is perfect if it contains n edges, which is clearly the maximum possible"
    * == all bases have to participate in base pairing
  * Noncrossing Matching: When we make the circle with a given RNA sequence, it is the collection of the edges that are not intersected
  * **Catalan Numbers**: counting noncrossing perfect matchings

### My Solution
* Sketch
  * All bases participate in base pairing
    * ∴ linear search → (seq[0] + seq[i]) is enough to play a role in a standard single base pair
  * In the circle seq
    * Use "Recursion" and "Combinatorics"
    * Let recursion function be func(seq)
    * **func(Left sequence) x 1 (standard base pair) x func(right sequence)**
    * Because it is perfect matching, the sequence as a parameter of func has to have even length
      * ∴ i has to be 1, 3, 5, 7, ....
  * Use "Dictionary" for "Dynamic Programming"
    * It remembers the catalan numbers of the sequence which has already been calculated

* Code

<br>

~~~python
matching_dic = {"" : 1, "AA" : 0, "AC" : 0, "AU" : 1, "AG" : 0, "CA" : 0, "CU" : 0, "CG" : 1, "CC" : 0, "UA" : 1, "UU" : 0, "UG" : 0, "UC" : 0, "GA" : 0, "GC" : 1, "GU" : 0, "GG" : 0}
def calculate_catalan(seq):
    if seq not in matching_dic:
        matching_dic[seq] = 0
        for i in range(1, len(seq), 2):
            matching_dic[seq] += matching_dic[seq[0] + seq[i]] * calculate_catalan(seq[1:i]) * calculate_catalan(seq[i + 1:])

    return matching_dic[seq] % 1000000        

seq = ""
dp = [{} for i in range(len(seq) // 2)]
name = input()
f = open(name, 'r')

f.readline()

for line in f.readlines():
    seq += line[:-1]

print(calculate_catalan(seq))

f.close()
~~~
