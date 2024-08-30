---
layout: post
title: Reversal Distance
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, BFS
tags: [rosalind problem]

categories:
  - study
  - rosalind
---

### Reversal Distance
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/rear/)

* Description
  * **Inversion**: "flips an entire interval of DNA found on the same chromosome"
  * Like calculating Hamming distance, we need to calculate the minimum number of doing inversion for making one sequence be same with the other sequence

### My Solution
* Sketch
  * [Reference Paper](https://link.springer.com/article/10.1007/BF01188586): **Exact and Approximation Algorithms for Computing Reversal Distances in Genome Rearrangement** (Kececioglu, J., & Sankoff, D. (1995). Exact and approximation algorithms for sorting by reversals, with application to genome rearrangement. Algorithmica, 13(1), 180-210. https://doi.org/10.1007/BF01188586 )
  * Let seq1 = (4, 1, 3, 2, 5), seq2 = (3, 5, 4, 2, 1)
    * Minimum number of inversion **(seq1 → seq2)** == Minimum number of inversion **(seq2<sup>-1</sup>●seq1 → ι)**
    * seq2<sup>-1</sup>: (Index, Value) → (Value, Index)
      * seq2: (1, 3), (2, 5), (3, 4), (4, 2), (5, 1)
      * seq2<sup>-1</sup>: (3, 1), (5, 2), (4, 3), (2, 4), (1, 5) == **(5, 4, 1, 3, 2)**
    * (seq1●seq2)[i] = seq1[seq2[i]] (i starts from 1)
      * (5, 4, 1, 3, 2)●(4, 1, 3, 2, 5) = **(3, 5, 1, 4, 2)**
    * ι == **(1, 2, 3, ..., n)**
  * How to calculate the minimum number of inversion **(seq2<sup>-1</sup>●seq1 → ι)**
    * "Breakpoint"
      * i with **abs(seq[i] - seq[i - 1]) != 1**
    * When we search the possible inversion case, we need to do it with the dirction of breakpoint decreasing
    * I use **BFS** for searching the possible inversion case
      * The node of the graph is (sequence from inversion, current number of inversion happening)

<br>    


* Code

<br>

~~~python
from collections import deque

def convertToInversionSeq(seqL):     # seq^(-1)
    res = [0] * (len(seqL)-2)
    res = [0] + res + [11]

    for i in range(1, len(seqL) - 1):
        res[i] = seqL.index(i)

    return res

def productByInversion(iseqL, seqL1):    # seq1 ● seq2
    res = [0] * (len(seqL1)-2)
    res = [0] + res + [11]

    for i in range(1, len(iseqL) - 1):
        res[i] = iseqL[seqL1[i]]

    return res

def calculate_breakpoint(seq):
    breakpoints = []

    for i in range(1, len(seq)):
        if abs(seq[i] - seq[i - 1]) != 1:
            breakpoints.append(i)

    return breakpoints

def calculate_reversal_distance(seq):
    need_v = deque()
    visited = set()
    need_v.append((seq, 0))
    visited.add(tuple(seq))

    while need_v:
        current_seq, current_inversion_count = need_v.popleft()

        if current_seq == [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]:
            return current_inversion_count

        current_breakpoints = calculate_breakpoint(current_seq)

        min_breakpoint_count = len(current_breakpoints)
        next_seqs = []

        for i in range(len(current_breakpoints)):
            for j in range(i + 1, len(current_breakpoints)):
                next_seq = current_seq[:current_breakpoints[i]] + current_seq[current_breakpoints[i]:current_breakpoints[j]][::-1] + current_seq[current_breakpoints[j]:]

                if tuple(next_seq) in visited:
                    continue
                next_breakpoint_count = len(calculate_breakpoint(next_seq))

                if next_breakpoint_count < min_breakpoint_count:
                    min_breakpoint_count = next_breakpoint_count
                    next_seqs = [next_seq]
                elif next_breakpoint_count == min_breakpoint_count:
                    next_seqs.append(next_seq)

        for i in range(len(next_seqs)):
            need_v.append((next_seqs[i], current_inversion_count + 1))
            visited.add(tuple(next_seqs[i]))           



name = input()
f = open(name, 'r')

seq1 = []
seq2 = []
turn = 1
temp = []
final_seq = []

for line in f.readlines():
    if line[0] == "\n":
        seq2 = convertToInversionSeq(seq2)
        final_seq = productByInversion(seq2, seq1)
        print(calculate_reversal_distance(final_seq), end = " ")
        continue
    else:
        if turn % 2 != 1:
            seq1 = [0] + list(map(int, line[:-1].split())) + [11]
            turn += 1
        else:
            seq2 = [0] + list(map(int, line[:-1].split())) + [11]
            turn += 1

seq2 = convertToInversionSeq(seq2)
final_seq = productByInversion(seq2, seq1)

print(calculate_reversal_distance(final_seq), end = " ")
f.close()
~~~
