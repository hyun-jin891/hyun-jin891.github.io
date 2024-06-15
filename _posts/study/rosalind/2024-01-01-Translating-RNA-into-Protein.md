---
layout: post
title: Translating RNA into Protein
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, String Algorithm
tags: [rosalind problem]

categories:
  - study
  - rosalind
---

### Translating RNA into Protein
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/prot/)

* Description
  * Given mRNA sequence (coding strand), we translate it to protein sequence (primary sequence)
* Input example

<br>

~~~
AUGGCCAUGGCGCCCAGAACUGAGAUCAAUAGUACCCGUAUUAACGGGUGA
~~~

<br>

* Output example

<br>

~~~
MAMAPRTEINSTRING
~~~

### Biological Background
* Codon table
  <br>
  ~~~python
  codon_table = {"UUU" : "F", "UUC" : "F", "UUA" : "L", "UUG" : "L", "UCU" : "S", "UCC" : "S", "UCA" : "S", "UCG" : "S", "UAU" : "Y", "UAC" : "Y", "UAA" :"", "UAG" : "", "UGU" : "C", "UGC" : "C", "UGA" : "", "UGG" : "W", "CUU" : "L", "CUC" : "L", "CUA" : "L", "CUG" : "L", "CCU": "P", "CCC" : "P", "CCA" : "P", "CCG" : "P", "CAU" : "H", "CAC" : "H", "CAA" : "Q", "CAG" : "Q", "CGU" : "R", "CGC" : "R", "CGA" : "R", "CGG" : "R", "AUU" : "I", "AUC" : "I", "AUA" : "I", "AUG" : "M", "ACU" : "T", "ACC" : "T", "ACA" : "T", "ACG" : "T", "AAU" : "N", "AAC" : "N", "AAA" : "K", "AAG" : "K", "AGU" : "S", "AGC" : "S", "AGA" : "R", "AGG" : "R", "GUU" : "V", "GUC" : "V", "GUA" : "V", "GUG" : "V", "GCU" : "A", "GCC" : "A", "GCA" : "A", "GCG" : "A", "GAU" : "D", "GAC" : "D", "GAA" : "E", "GAG" : "E", "GGU" : "G", "GGC" : "G", "GGA" : "G", "GGG" : "G"}
  ~~~

### My Solution
<br>

~~~python
def translate(seq):
    codon_table = {"UUU" : "F", "UUC" : "F", "UUA" : "L", "UUG" : "L", "UCU" : "S", "UCC" : "S", "UCA" : "S", "UCG" : "S", "UAU" : "Y", "UAC" : "Y", "UAA" :"", "UAG" : "", "UGU" : "C", "UGC" : "C", "UGA" : "", "UGG" : "W", "CUU" : "L", "CUC" : "L", "CUA" : "L", "CUG" : "L", "CCU": "P", "CCC" : "P", "CCA" : "P", "CCG" : "P", "CAU" : "H", "CAC" : "H", "CAA" : "Q", "CAG" : "Q", "CGU" : "R", "CGC" : "R", "CGA" : "R", "CGG" : "R", "AUU" : "I", "AUC" : "I", "AUA" : "I", "AUG" : "M", "ACU" : "T", "ACC" : "T", "ACA" : "T", "ACG" : "T", "AAU" : "N", "AAC" : "N", "AAA" : "K", "AAG" : "K", "AGU" : "S", "AGC" : "S", "AGA" : "R", "AGG" : "R", "GUU" : "V", "GUC" : "V", "GUA" : "V", "GUG" : "V", "GCU" : "A", "GCC" : "A", "GCA" : "A", "GCG" : "A", "GAU" : "D", "GAC" : "D", "GAA" : "E", "GAG" : "E", "GGU" : "G", "GGC" : "G", "GGA" : "G", "GGG" : "G"}    
    res = ""
    for i in range(0, len(seq) - 2, 3):
        codon = seq[i : i + 3]
        res += codon_table[codon]

    return res

file_name = input()
f = open(file_name, 'r')

seq = f.readline()

print(translate(seq))
~~~

<br>

* We can make it using Biopython
<br>

~~~python
from Bio.Seq import Seq

s = Seq("AUGGCCAUGGCGCCCAGAACUGAGAUCAAUAGUACCCGUAUUAACGGGUGA")
p = s.translate()
print(p)
~~~
<br>
output

~~~
MAMAPRTEINSTRING*
~~~
