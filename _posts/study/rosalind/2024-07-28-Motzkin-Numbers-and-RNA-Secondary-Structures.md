---
layout: post
title: Motzkin Numbers and RNA Secondary Structures
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Dictionary, Recursion, String Algorithm, Combinatorics, Dynamic Programming
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Motzkin Numbers and RNA Secondary Structures
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/motz/)

* Description
  * Matching: "a collection of edges that no node belongs to more than one edge in the collection"
    * == one base can form only one base pair
  * Perfect Matching: "If graph contains an even number of nodes (say 2n ), then a matching on the graph is perfect if it contains n edges, which is clearly the maximum possible"
    * == all bases have to participate in base pairing
  * Noncrossing Matching: When we make the circle with a given RNA sequence, it is the collection of the edges that are not intersected
  * Catalan Numbers: counting noncrossing perfect matchings
  * Perfect Matching X
    * Some bases do not participate in base pairing
    * count(A) != count(T) & count(G) != count(C)
    * We have to calculate the number of maximum matchings
      * Maximum matching: "forming as many base pairs as possible in an RNA string"
  * **Motzkin number**: "counts the number of ways to form a (not necessarily perfect) noncrossing matching"

### My Solution
* Sketch
  * Some bases don't participate in base pairing
    * Do linear search for (seq[**0**] + seq[i])
    * seq[0] gonna be different base because the sequence gonna be different whenever we call recursion
  * In the circle seq
    * Use "Recursion" and "Combinatorics"
    * Let recursion function be func(seq)
    * During the linear search, we first count the number of cases (경우의 수) for the case that seq[0] forms base pair with seq[i]
      * Number of cases (경우의 수) for the case that seq[0] forms base pair with seq[i]
        * **func(Left sequence) x 1 (seq[0] & seq[i] base pair) x func(right sequence)** (Go to "Catalan Numbers posting")
        * Each number of cases participates in the addition principle (합의 법칙)
    * We need to consider the case that we don't count the seq[0]'s base pairing
      * **func(seq[1:])**: This participates in the addition principle  
    * Use "Dictionary" for "Dynamic Programming"
      * It remembers the motzkin numbers of the sequence which has already been calculated


* Code

<br>

~~~python
matching_dic = {"" : 1, "A" : 1, "U" : 1, "G" : 1, "C" : 1, "AA" : 1, "AC" : 1, "AU" : 2, "AG" : 1, "CA" : 1, "CU" : 1, "CG" : 2, "CC" : 1, "UA" : 2, "UU" : 1, "UG" : 1, "UC" : 1, "GA" : 1, "GC" : 2, "GU" : 1, "GG" : 1}

def calculate_num(seq):
    if seq not in matching_dic:
        matching_dic[seq] = 0
        for i in range(1, len(seq)):
            if matching_dic[seq[0] + seq[i]] == 1:
                continue
            else:
                subseq1 = seq[1 : i]
                subseq2 = seq[i + 1:]

                res1 = calculate_num(subseq1)
                res2 = calculate_num(subseq2)

                matching_dic[seq] += (res1 * res2) % 1000000

        matching_dic[seq] += calculate_num(seq[1:])        


    return (matching_dic[seq]) % 1000000


seq = ""

name = input()
f = open(name, 'r')

f.readline()

for line in f.readlines():
    seq += line[:-1]

print((calculate_num(seq)) % 1000000)

f.close()
~~~

* Code Analysis

<br>

~~~python
matching_dic = {"" : 1, "A" : 1, "U" : 1, "G" : 1, "C" : 1, "AA" : 1, "AC" : 1, "AU" : 2, "AG" : 1, "CA" : 1, "CU" : 1, "CG" : 2, "CC" : 1, "UA" : 2, "UU" : 1, "UG" : 1, "UC" : 1, "GA" : 1, "GC" : 2, "GU" : 1, "GG" : 1}
~~~

1: empty set (공집합)<br>
2: empty set + one base pair case <br>

------

<br>

~~~python
def calculate_num(seq):
    if seq not in matching_dic:
        matching_dic[seq] = 0
        for i in range(1, len(seq)):
            if matching_dic[seq[0] + seq[i]] == 1:
                continue
            else:
                subseq1 = seq[1 : i]
                subseq2 = seq[i + 1:]

                res1 = calculate_num(subseq1)
                res2 = calculate_num(subseq2)

                matching_dic[seq] += (res1 * res2) % 1000000

        matching_dic[seq] += calculate_num(seq[1:])        


    return (matching_dic[seq]) % 1000000
~~~

count the number of cases for the case that seq[0] forms base pair with seq[i] → calculate_num(Left sequence) x 1 (seq[0] & seq[i] base pair) x calculate_num(right sequence)<br>

We need to consider the case that we don't count the seq[0]'s base pairing → **func(seq[1:])**

------
