---
layout: post
title: Finding a Shared Motif
description: >
  A problem from rosalind "Bioinformatics Stronghold" category
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Finding a Shared Motif
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/lcsm/)

* Description
  * If we know the motif sequence, we can find the location of that motif on a suggested sequence with Boyer-Moore algorithm
  * If we don't know the motif sequence, we can guess the motif sequence by finding the common string (seqence) between various sequences
  * This problem asks us to get **the longest common substring** (Because it is "substring", we don't need to consider mutation (we only focus on the continuous string))


### My Solution
<br>

~~~python
def findSubSeq(seq, mLength):
    n = 0
    for i in range(mLength):
        n += len(seq) - i

    res = [0 for i in range(n)]
    index = 0

    for i in range(mLength, 0, -1):
        for j in range(len(seq)):
            if j + i > len(seq):
                break
            res[index] = seq[j : j + i]
            index += 1

    return res

def preProcessBCR(pattern):
    BCR_dictionary = {'A' : 0, 'G' : 0, 'C' : 0, 'T' : 0}

    for i in range(len(pattern)):
        character = pattern[i]
        BCR_dictionary[character] = i

    return BCR_dictionary

def preProcessGSR(pattern):
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
    for j in range(len(shift)):
        if shift[j] == 0:
            shift[j] = i
        if j == i:
            i = bpos[j]

    return shift

def BoyerMoore(seq, pattern, BCR_dictionary, GSR_shift):
    i = 0

    while i <= len(seq) - len(pattern):
        j = len(pattern) - 1
        while j >= 0 and pattern[j] == seq[i + j]:
            j -= 1

        if j < 0:
            return True
        else:
            i += max(GSR_shift[j + 1], j - BCR_dictionary[seq[i + j]])

    return False


name = input()
f = open(name, 'r')
count = 1
seqs = []

for line in f.readlines():
    if line[0] == ">":
        if count == 1:
            resultSeq = ""
            count += 1
        else:
            seqs.append(resultSeq)
            resultSeq = ""   
        continue
    else:
        resultSeq += line[:-1]

seqs.append(resultSeq)

minLength = len(seqs[0])

for i in range(len(seqs)):
    if minLength > len(seqs[i]):
        minLength = len(seqs[i])

motifs = findSubSeq(seqs[0], minLength)


for i in range(len(motifs)):
    motif = motifs[i]
    bcr_dictionary = preProcessBCR(motif)
    gsr_shift = preProcessGSR(motif)
    flag = True
    for j in range(1, len(seqs)):
        seq = seqs[j]
        if BoyerMoore(seq, motif, bcr_dictionary, gsr_shift) == False:
            flag = False
            break

    if flag:
        print(motif)
        print(len(motif))
        break
~~~

<br>

* Sketch
  * 1<sup>st</sup> : I extract the motifs from first input sequence (findSubSeq)
    * **Critical point 1** : Candidate motif's length <= minimum length of input sequences
      * Before calling findSubSeq, I find the minimum length of input sequences
    * **Critical point 2** : For efficient calculation -> When I extract the motif and store it to the list, I get the motif with maximum length to enter the list at first (after this, maxLength-1 -> maxLength-2, maxLength-3, ...)
      * motif's maximum length == minimum length of input sequences
      * Because we have to get the **longest** common substring, I store the candidate motifs with descending order
  * 2<sup>nd</sup> : While doing linear search on the input sequences with the linear search on the candidate motifs' list, I check whether current motif is in current sequence through Boyer-Moore algorithm
    * I modify the Boyer-Moore algorithm : If there is the current motif at least one time, return True
    * Because I store the candidate motifs with descending order, the motif that doesn't make the Boyer-Moore algorithm give "False" at all during the loop at first is the longest common substring
