---
layout: post
title: Boyer-Moore Algorithm
description: >
  For searching motifs in suggested sequences
tags: [Motifs]

categories:
  - study
  - rosalind
---

### Boyer-Moore Algorithm

* We should find the patterns which are represented in the suggested sequences
* There are various methods for it
* Boyer-Moore algorithm is efficient for searching patterns in string
* It should use two methods simultaneously
  * Bad-character rule (BCR)
  * Good suffix rule (GSR)
* It efficiently decreases the searching path in comparison to Brute-force algorithm

### BCR
* Example<br>
A C G G T C A T G C C A (Suggested Seq)<br>
A T C A G (Pattern)<br>

* First of all, we should identify the location where there is a mismatch which is the closest to the end of the pattern (T-G)
* We should focus on a symbol (T) in suggested seq which is involved in that mismatch
* We should find the location of the symbol in pattern. (If there are the symbols more than one, we choose one which is the closest to the end of the pattern)
  * Consequently, it is possible to move backward (that is, we need more rule (: GSR) for efficently finding the searching path)
* We move the pattern<br>
A C G G T C A T G C C A<br>
...........A T C A G<br>
* If the symbol is not in pattern, we just move the pattern by the length of pattern
* For this calculation, we should do preprocessing
  * We should make the dictionary (BCR_Dictionary) representing each nucleotides' index which is the closest to the end of pattern
  * If pattern is ATCAG, BCR_Dictionary = {'A' : 3, 'T' : 1, 'C' : 2, 'G' : 4}
  * If a certain nucleotide is not in pattern, the value in BCR_Dictionary is -1 (for calculating the case that the symbol is not in pattern : we just move the pattern by the length of pattern)

### GSR
* Good suffix : From the end of the pattern to the location where next location is first mismatch<br>

A C G G T C A T G C C A<br>
T G T G T<br>

Good suffix = G T

* GSR-Case 1 : There is one more sequence in the pattern which is equal to good suffix<br>

A C G G T C A T G C C A<br>
T G T G T<br>

→ <br>

A C G G T C A T G C C A<br>
........T G T G T<br>

* GSR-Case 2: There is sequence in the pattern which is the part of good suffix<br>

A C G G T C A T G C C A<br>
C A T A T C A<br>

→ <br>

A C G G T C A T G C C A<br>
..................C A T A T C A<br>

* For this calculation, we should do preprocessing
  * We should make the list (shift[])
  * shift[n] : Assume the good suffix is from n (index) to the end of pattern → list value = the number needed for pattern shifting
    * **index n-1 is absolutely mismatch** (definition of good suffix)
    * we don't know whether 1 ~ n-2 is mismatch or not
  * For calculating shift[], we should make the list (bpos[])
    * bpos[n] : Assume there is a string from n (index) to the end of pattern → list value = start index of suffix which is equal to prefix
    * Example : ABABA → bpos = 2 (prefix = suffix = ABA)

### preProcessingBCR

~~~python
def preProcessingBCR(pattern):
  BCR_Dictionary = {}
  Nucleotide = ['A', 'G', 'C', 'T']

  for nt in Nucleotide:
    BCR_Dictionary[nt] = -1

  for nt in range(len(pattern)):
    BCR_Dictionary[pattern[nt]] = nt
~~~
<br>

### preProcessingGSR

~~~python
def preProcessingGSR(pattern):
  bpos = [0] * (len(pattern) + 1)
  shift = [0] * (len(pattern) + 1)

  i = len(pattern)
  j = len(pattern) + 1
  bpos[i] = j

  while i > 0:
    while j <= len(pattern) and pattern[i - 1] != pattern[j - 1]:
      if shift[j] == 0:
        shift[j] = j - i
      j = bpos[j]

    i -= 1
    j -= 1
    bpos[i] = j

  i = bpos[0]
  for j in range(len(pattern)):
    if shift[j] == 0:
      shift[j] = i
    if j == i:
      i = bpos[i]
~~~
<br>

### Implementation for Boyer-Moore

~~~python
def BoyerMoore(pattern, seq, BCR_Dictionary, shift):
  i = 0
  result = []

  while i <= len(seq) - len(pattern):
    j = len(pattern) - 1

    while j >= 0 and pattern[j] == seq[i + j]:
      j -= 1

    if j < 0:
      result.append(i)
      i += shift[0]
    else:
      i += max(shift[j + 1], j - BCR_Dictionary[seq[i + j]])

  return result
~~~
