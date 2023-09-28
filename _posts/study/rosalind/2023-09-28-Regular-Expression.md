---
layout: post
title: Regular Expression
description: >
  For searching motifs in suggested sequences
tags: [Motifs]

categories:
  - study
  - rosalind
---

### Regular Expression

* We should find the patterns which are represented in the suggested sequences
* There are various methods for it
* Regular Expression is simple for it
* However, it is not efficient when there are lots of data
* After defining the regular expression, we gonna find the pattern which corresponds to it
  * Ex) G[CG]A (RE) → GCA, GGA

### Symbol representing RE
* . : any character
  * Ex) A.G → ACG, AGG, AAG, ATG, ....
* chr? : X or chr * 1
  * Ex) A?G → G, AG
* chr* : X or chr * n (n >= 1)
  * Ex) A*G → G, AG, AAG, AAAG, ....
* chr+ : chr * n (n >= 1)
  * Ex) A+G → AG, AAG, AAAG, ....
* [chr1chr2chr3...] : one from [chr1, chr2, chr3, ....]
  * Ex) [AC]G → AG, CG
  * [num1num2num3...] : one from [num1, num2, num3, ....]
  * [ACTGactg]* : all DNA sequences
* [^chr1chr2chr3...] : one from (chr1, chr2, chr3, ...)<sup>C</sup>
* chr{n} : chr * n
  * Ex) GCA(TG){3} → GCATGTGTG
* chr{n, m} : chr * α (n <= α <= m)
  * Ex) AC{1, 2}G → ACG, ACCG
* seq1 | seq2 : seq1 or seq2
  * Ex) ATT | A(CG){2} → ATT, ACGCG
* M[ ^_ ]*_ : coding amino acid sequence (_ : stop codon)

### Module for RE
* import re
* re.search(RE, seq) : return object representing first subsequence corresponding to RE from sequence
  * object.group() : set of subsequence
    * RE = "(seq1)(seq2)"
    * group(1) : In group(), set of seq1
    * group(2) : In group(), set of seq2
  * object.span() : (start_index, end_index + 1)
* re.match(RE, seq) : return object representing whether first subsequence (start from index 0) from sequence corresponds to RE
* re.findall(RE, seq) : return list representing all subsequence corresponding to RE
  * element from list is not object
* re.finditer(RE, seq) : return iterator representing all subsequence corresponding to RE<br>
  * element from iterator is objcet : using group(), span()

~~~python
import re

seq = "ACACACCCGGCGCGAGCATCGTCACTGCAGCATCGACTCCTCGAGCACGTTCTCCACCGTTTCACTCACTATCGG"

regexp = "(ACA)(C.C)"

motifs = re.finditer(regexp, seq)

for motif in motifs:
  print(motif.group(), end=' ')
  print(motif.span(), end=' ')
  print(motif.group(1), end='')
~~~
<br>
* result: ACACAC (0, 6) ACA

### Lookahead Assertion
* 전방 탐색 어설션
* There are some problems when we use re.findall (or finditer)
  * If there are overlaps, it cannot distinguish them
  * Ex) seq = "GAGAC", RE = "GA[GC]"
    * desired result = GAG (start : 0), GAC (start : 2)
    * result = GAG because of the overlap
* Lookahead Assertion
  * If the form of RE gonna be "(?= original RE)", it can distinguish overlaps
  * However, it knows only start index (not end index, so it will return just start index no matter how you use span())
  * Both group() (return nothing) and span() ((start index, start index)) are changed so you cannot use them for original purpose, but the function of group(n) remains<br>
  * After slicing the sequence based on start index calculated from lookahead assertion, calculate again with original RE


~~~python
import re

seq = "ACACACCCGGCGCGAGCATCGTCACTGCAGCATCGACTCCTCGAGCACGTTCTCCACCGTTTCACTCACTATCGG"

regexp = "(?=" + "(ACA)(C.C)" + ")"

motifs = re.finditer(regexp, seq)

for motif in motifs:
  print(motif.group(), end=' / ')
  print(motif.span(), end=' / ')
  print(motif.group(1), end=' / ')

~~~
<br>

* result : / (0, 0) / ACA /  / (2, 2) / ACA /

### Preprocessing
* Before using method from module re, set the RE inside it so that it processes for efficient calculation based on the specific RE
* use compile method
  * re.compile(RE)
<br>

~~~python
import re
import time

seq = "ACACACCCGGCGCGAGCATCGTCACTGCAGCATCGACTCCTCGAGCACGTTCTCCACCGTTTCACTCACTATCGG"

start = time.time()

regexp = "(ACA)(C.C)"
motifs = re.finditer(regexp, seq)

for motif in motifs:
  print(motif.group(), end=' ')

end = time.time()
print(f"{end - start:.5f} sec", end=' ')

start = time.time()

compileRE = re.compile(regexp)

motifs = compileRE.finditer(seq)

for motif in motifs:
  print(motif.group(), end=' ')

end = time.time()
print(f"{end - start:.5f} sec")
~~~
<br>

* result : ACACAC 0.00126 sec ACACAC 0.00000 sec

### Database
* Data from database → convert to RE → search it from the suggested sequence
* Example : [Prosite](https://prosite.expasy.org/)
* The form of motif from Prosite is different compared to that of RE
  * "-" exists between two symbols
  * "x" : any amino acid
  * "()" : equal to "{}" from RE
  * Ex) arsR family consensus pattern
  * C-x(2)-D-[LIVM]-x(6)-[ST]-x(4)-S-[HYR]-[HQ]
<br>

~~~python
def convert(data):
  data.replace("-", "")
  data.replace("x", ".")
  data.replace("(", "{")
  data.replace(")", "}")

  return data
~~~
