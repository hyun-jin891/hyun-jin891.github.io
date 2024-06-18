---
layout: post
title: Inferring mRNA from Protein
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Dictionary, Module
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Inferring mRNA from Protein
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/mrna/)

* Description
  * "Although any RNA string can be translated into a unique protein string, reversing the process yields a huge number of possible RNA strings from a single protein string because most amino acids correspond to multiple RNA codons"
  * For a matter of computer memory, we need to save the big data through calculating it with Modular arithmetic (Go to **"Remainder"** posting within "Computer Science" category)

### My Solution
* Sketch
  * Make codon_table dictionary
    * Its keys are a specific amino acid
    * Its values are the number of codons encoding this amino acid

* Code

<br>

~~~python
codon_table = {"F" : 2, "L" : 6, "I" : 3, "M" : 1, "V" : 4, "S" : 6, "P" : 4, "T" : 4, "A" : 4, "Y" : 2, "H" : 2, "Q" : 2, "N" : 2, "K" : 2, "D" : 2, "E" : 2, "C" : 2, "W" : 1, "R" : 6, "G" : 4}

name = input()
f = open(name, 'r')

aaSeq = ""

for line in f.readlines():
    aaSeq += line[:-1]


case = 1

for i in range(len(aaSeq)):
    aa = aaSeq[i]
    case *= codon_table[aa]
    case %= 1000000

print((case * 3) % 1000000)
~~~
