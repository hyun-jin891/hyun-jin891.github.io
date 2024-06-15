---
layout: post
title: Consensus and Profile
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, String Algorithm
tags: [rosalind problem]

categories:
  - study
  - rosalind
---

### Consensus and Profile
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/cons/)

* Description
  * Given several FASTA format data, make the profile and find the consensus
* Input example

<br>

~~~
>Rosalind_1
ATCCAGCT
>Rosalind_2
GGGCAACT
>Rosalind_3
ATGGATCT
>Rosalind_4
AAGCAACC
>Rosalind_5
TTGGAACT
>Rosalind_6
ATGCCATT
>Rosalind_7
ATGGCACT
~~~

<br>

* Output example

<br>

~~~
ATGCAACT
A: 5 1 0 0 5 5 0 0
C: 0 0 1 4 2 0 6 1
G: 1 1 6 3 0 1 0 0
T: 1 5 0 0 0 1 1 6
~~~

### Biological Background
* We want to analyze & compare several sequences (same length)
  * They can be motifs from several organisms
* Making the Profile can make it easy to analyze these
  * **Profile** : Each symbol's frequency on i<sup>th</sup> location (4 x (length of sequence) matrix)
* **Consensus** : sequence of symbols that have maximum frequency on each location

### My Solution

<br>

~~~python
nucleotides = {'A' : 0, 'C' : 1, 'G' : 2, 'T' : 3}
reverse_nt = {0 : 'A', 1 : 'C', 2 : 'G', 3 : 'T'}
def create_profile_consensus(seqs):
    res1 = [[0 for i in range(len(seqs[0]))] for i in range(4)]
    res2 = ""

    for i in range(len(seqs[0])):
        for j in range(len(seqs)):           
            nt_index = nucleotides[seqs[j][i]]
            res1[nt_index][i] += 1
    for i in range(len(seqs[0])):
        max_symbol = reverse_nt[0]
        max_freq = res1[0][i]
        for j in range(4):
            if max_freq < res1[j][i]:
                max_symbol = reverse_nt[j]
                max_freq = res1[j][i]
        res2 += max_symbol


    return res1, res2


file_name = input()
f = open(file_name, 'r')


Seqs = []
res = ""
for line in f.readlines():
    if line[0] == ">":
        if res != "":
            Seqs.append(res)
            res = ""
        continue
    else:
        res += line[:-1]

Seqs.append(res)

profile, consensus = create_profile_consensus(Seqs)

print(consensus)

for i in range(len(profile)):
    print(reverse_nt[i] + ":", end="")
    for j in range(len(profile[i])):
        print(" " + str(profile[i][j]), end="")
    print()
~~~

<br>

* Code Analysis

<br>

~~~python
def create_profile_consensus(seqs):
    res1 = [[0 for i in range(len(seqs[0]))] for i in range(4)]
    res2 = ""

    for i in range(len(seqs[0])):
        for j in range(len(seqs)):           
            nt_index = nucleotides[seqs[j][i]]
            res1[nt_index][i] += 1
    for i in range(len(seqs[0])):
        max_symbol = reverse_nt[0]
        max_freq = res1[0][i]
        for j in range(4):
            if max_freq < res1[j][i]:
                max_symbol = reverse_nt[j]
                max_freq = res1[j][i]
        res2 += max_symbol


    return res1, res2
~~~

<br>

res1 : profile<br>
res2 : consensus sequence<br>

<br>

~~~python
file_name = input()
f = open(file_name, 'r')


Seqs = []
res = ""
for line in f.readlines():
    if line[0] == ">":
        if res != "":
            Seqs.append(res)
            res = ""
        continue
    else:
        res += line[:-1]

Seqs.append(res)
~~~

Distinguish between "> ID" and sequence<br>
line[-1] == '\n'<br>

<br>

~~~python
profile, consensus = create_profile_consensus(Seqs)

print(consensus)

for i in range(len(profile)):
    print(reverse_nt[i] + ":", end="")
    for j in range(len(profile[i])):
        print(" " + str(profile[i][j]), end="")
    print()
~~~

print out the consensus and profile<br>

### Best answer
[answer](https://rosalind.info/problems/cons/explanation/)<br>
