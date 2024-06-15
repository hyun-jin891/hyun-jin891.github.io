---
layout: post
title: Computing GC Content
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, String Algorithm
tags: [rosalind problem]

categories:
  - study
  - rosalind
---

### Computing GC Content
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/gc/)

* Description
  * Given a Multi-FASTA file, we should choose a sequence which has the highest GC content
* Input example

<br>

~~~
>Rosalind_6404
CCTGCGGAAGATCGGCACTAGAATAGCCAGAACCGTTTCTCTGAGGCTTCCGGCCTTCCC
TCCCACTAATAATTCTGAGG
>Rosalind_5959
CCATCGGTAGCGCATCCTTAGTCCAATTAAGTCCCTATCCAGGCGCTCCGCCGAAGGTCT
ATATCCATTTGTCAGCAGACACGC
>Rosalind_0808
CCACCCTCGTGGTATGGCTAGGCATTCAGGAACCGGAGAACGCTTCAGACCAGCCCGGAC
TGGGAACCTGCGGGCAGTAGGTGGAAT
~~~

<br>

* Output example

<br>

~~~
Rosalind_0808
60.919540
~~~

### Biological Background
* FASTA file
  * [Definition](https://www.incodom.kr/FASTA)
* "In practice, the GC-content of most eukaryotic genomes hovers around 50%. However, because genomes are so long, we may be able to distinguish species based on very small discrepancies in GC-content; furthermore, most prokaryotes have a GC-content significantly higher than 50%, so that GC-content can be used to quickly differentiate many prokaryotes and eukaryotes by using relatively small DNA samples." - Rosalind (Computing GC Content)

### My Solution
<br>

~~~python
def GC_Calculator(seq):
    length = len(seq)
    GC_count = 0

    for s in seq:
        if s == 'G' or s == 'C':
            GC_count += 1

    return (GC_count / length) * 100

name = input()
f = open(name, 'r')

max_ID = ""
max_value = 0
test = 0
last_seq = ""
last_ID = ""


for line in f.readlines():
    if test == 0:
        seq = ""
        ID = ""
        test += 1             ### Initialization

    if line[0] == ">":        ### FASTA begins
        if seq != "":         ### Except first seq
            value = GC_Calculator(seq)
            seq = ""
            if value > max_value:
                max_value = value
                max_ID = ID
        ID = line[1 : -1]
    else:
        seq += line[:-1]

    last_seq = seq
    last_ID = ID             ### "readlines" won't calculate last sequence

last_value = GC_Calculator(last_seq)
if last_value > max_value:
    max_value = last_value    
    max_ID = last_ID
print(max_ID)
print(max_value)
~~~

<br>
