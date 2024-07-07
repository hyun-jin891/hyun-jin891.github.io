---
layout: post
title: RNA Splicing
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Regular Expression, String Algorithm
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### RNA Splicing
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/splc/)

* Description
  * pre-mRNA involves several introns that are not coding region
  * RNA splicing -> remove the introns    

### My Solution
* Sketch
  * This rosalind problem intends for us not to consider the overlaps of several intron sequences (A given dataset, there is no such a case)
    * **I am curious there can be such a case in the real world**
      * If there can be, is it right that only one of overlapped intron sequences is removed? (2024-07-07 My question)
      * I need to study more when it comes to it
      * I don't need to consider such a case in this rosalind problem, but I consider that case as well with my thought: "Only one of overlapped intron sequences is removed"

      ~~~
      Intron: ACCT, CTTA

      5-...ATGACCTTACAGCGGATTAG...-3
              ACCT
                CTTA

      Possible Result 1: ATGTACAGCGGATTAG
      Possible Result 2: ATGACCAGCGGATTAG      

    ~~~

  * Get the information about intron's start location and end location within a given DNA sequence through regular expression (ori_intron)
  * Through "Lookahead Assertion" and string algorithm, I get the overlapped introns (over_intron) and I am tracking which introns (ori_intron) go through overlapping with these introns (over_intron)
  * If there is no overlap, we just remove introns (ori_intron) from the DNA sequence
  * If there are overlapped sequences
    * Which one do we remove between ori_intron and over_intron: This is the matter of combination (That is, it is the matter of whether one intron is selected or not)
    * I use Brute-force algorithm (exhaustive search, 완전탐색) through bitmask
    * For all possible combination cases, I get the sequences where I removed the introns satisfying the current combination
  * All possible final sequences -> Translate

* Code

<br>

~~~python
import re
codonTable = {"TTT" : "F", "TTC" : "F", "TTA" : "L", "TTG" : "L", "CTT" : "L", "CTC" : "L", "CTA" : "L", "CTG" : "L", "ATT" : "I", "ATC" : "I", "ATA" : "I", "ATG" : "M", "GTT" : "V", "GTC" : "V", "GTA" : "V", "GTG" : "V", "TCT" : "S", "TCC" : "S", "TCA" : "S", "TCG" : "S", "CCT" : "P", "CCC" : "P", "CCA" : "P", "CCG" : "P", "ACT" : "T", "ACC" : "T", "ACA" : "T", "ACG" : "T", "GCT" : "A", "GCC" : "A", "GCA" : "A", "GCG" : "A", "TAT" : "Y", "TAC" : "Y", "CAT" : "H", "CAC" : "H", "CAA" : "Q", "CAG" : "Q", "AAT" : "N", "AAC" : "N", "AAA" : "K", "AAG" : "K", "GAT" : "D", "GAC" : "D", "GAA" : "E", "GAG" : "E", "TGT" : "C", "TGC" : "C", "TGG" : "W", "CGT" : "R", "CGC" : "R", "CGA" : "R", "CGG" : "R", "AGT" : "S", "AGC" : "S", "AGA" : "R", "AGG" : "R", "GGT" : "G", "GGC" : "G", "GGA" : "G", "GGG" : "G"}

def translate(seq):
    startCodon = "(?=" + "ATG" + ")"
    compiler = re.compile(startCodon)
    startCodonLocations = compiler.finditer(seq)
    startCodonList = []


    for i in startCodonLocations:
        startCodonList.append(i.span()[0])

    protein = ""
    flag = False
    for i in range(len(startCodonList)):
        if (flag):
            break
        for j in range(startCodonList[i], len(seq), 3):
            codon = seq[j : j + 3]
            if codon not in codonTable.keys():
                flag = True
                break
            else:
                protein += codonTable[codon]
    return protein

def quick_sort(array):
    if len(array) <= 1:
        return array

    pivot = len(array) // 2
    front_arr, pivot_arr, rear_arr = [], [], []

    for value in array:
        if value.getX() < array[pivot].getX():
            front_arr.append(value)
        elif value.getX() > array[pivot].getX():
            rear_arr.append(value)
        else:
            pivot_arr.append(value)
    if len(front_arr) == 0 and len(rear_arr) == 0 and len(pivot_arr) != 0:
        return pivot_arr
    return quick_sort(front_arr) + quick_sort(pivot_arr) + quick_sort(rear_arr)

name = input()
f = open(name, 'r')
f.readline()
count = 0
seq = ""
reExp = []
input_intron = ""

for line in f.readlines():
    if line[0] != '>':
        if count == 0:
            seq += line[:-1]
        else:
            input_intron += line[:-1]

    else:
        if count == 0:
            count += 1
        else:
            reExp.append(input_intron)
            input_intron = ""

reExp.append(input_intron)


candidate_seqs = []
ori_intron_location = []
overlap_intron_location = []

class Intron:
    def __init__(self, x, y):
        self.__x = x
        self.__y = y
        self.__pair = []

    def getX(self):
        return self.__x
    def getY(self):
        return self.__y
    def getPair(self):
        return self.__pair
    def appendPair(self, other):
        self.__pair.append(other)
    def __eq__(self, other):
        return self.__x == other.__x and self.__y == other.__y

for i in range(len(reExp)):
    oneReExp = reExp[i]
    compiler = re.compile(oneReExp)
    introns = compiler.finditer(seq)

    for intron in introns:
        x = intron.span()[0]
        y = intron.span()[1]

        newIntron = Intron(x, y)
        ori_intron_location.append(newIntron)

for i in range(len(reExp)):
    oneReExp = "(?=" + reExp[i] + ")"
    compiler2 = re.compile(oneReExp)
    introns = compiler2.finditer(seq)

    for intron in introns:
        x = intron.span()[0]
        y = x + len(reExp[i])


        if Intron(x, y) in ori_intron_location:
            continue
        else:
            newIntron = Intron(x, y)
            overlap_intron_location.append(newIntron)

removedOriIntron = []
for i in range(len(ori_intron_location)):
    ori_intron1 = ori_intron_location[i]
    for j in range(i, len(ori_intron_location)):
        if i == j:
            continue
        else:
            ori_intron2 = ori_intron_location[j]
            if (ori_intron2.getX() >= ori_intron1.getX() and ori_intron2.getX() <= ori_intron1.getY() - 1) or (ori_intron2.getY() >= ori_intron1.getX() and ori_intron2.getY() <= ori_intron1.getY() - 1):
                removedOriIntron.append(ori_intron2)
                overlap_intron_location.append(ori_intron2)
new_ori_intron_location = []               
for i in range(len(ori_intron_location)):
    if ori_intron_location[i] not in removedOriIntron:
        new_ori_intron_location.append(ori_intron_location[i])



for i in range(len(overlap_intron_location)):
    overlap_intron = overlap_intron_location[i]
    for j in range(len(new_ori_intron_location)):
        new_ori_intron = new_ori_intron_location[j]

        if (overlap_intron.getX() >= new_ori_intron.getX() and overlap_intron.getX() <= new_ori_intron.getY() - 1) or (overlap_intron.getY() >= new_ori_intron.getX() and overlap_intron.getY() <= new_ori_intron.getY() - 1):
            overlap_intron.appendPair(new_ori_intron)


for i in range(2 ** len(overlap_intron_location)):
    possibleSeq = ""
    removed_intron = []
    pre_ori_intron = -1
    for j in range(len(overlap_intron_location)):
        if i & (1 << j) != 0:
            for k in range(len(overlap_intron_location[j].getPair())):
                removed_intron.append(overlap_intron_location[j].getPair()[k])
        else:
            removed_intron.append(overlap_intron_location[j])

    new_ori_intron_location = new_ori_intron_location + overlap_intron_location
    new_ori_intron_location = quick_sort(new_ori_intron_location)          

    for l in range(len(new_ori_intron_location)):
        if new_ori_intron_location[l] in removed_intron:
            continue
        else:
            if pre_ori_intron != -1:
                possibleSeq += seq[pre_ori_intron : new_ori_intron_location[l].getX()]


            else:
                possibleSeq += seq[: new_ori_intron_location[l].getX()]


            pre_ori_intron = new_ori_intron_location[l].getY()

    possibleSeq += seq[pre_ori_intron :]

    candidate_seqs.append(possibleSeq)



proteins = []
for i in range(len(candidate_seqs)):
    proteins.append(translate(candidate_seqs[i]))

for i in range(len(proteins)):
    print(proteins[i])

f.close()
~~~

* Code Analysis

~~~python

def translate(seq):
    startCodon = "(?=" + "ATG" + ")"
    compiler = re.compile(startCodon)
    startCodonLocations = compiler.finditer(seq)
    startCodonList = []


    for i in startCodonLocations:
        startCodonList.append(i.span()[0])

    protein = ""
    flag = False
    for i in range(len(startCodonList)):
        if (flag):
            break
        for j in range(startCodonList[i], len(seq), 3):
            codon = seq[j : j + 3]
            if codon not in codonTable.keys():
                flag = True
                break
            else:
                protein += codonTable[codon]
    return protein

def quick_sort(array):
    if len(array) <= 1:
        return array

    pivot = len(array) // 2
    front_arr, pivot_arr, rear_arr = [], [], []

    for value in array:
        if value.getX() < array[pivot].getX():
            front_arr.append(value)
        elif value.getX() > array[pivot].getX():
            rear_arr.append(value)
        else:
            pivot_arr.append(value)
    if len(front_arr) == 0 and len(rear_arr) == 0 and len(pivot_arr) != 0:
        return pivot_arr
    return quick_sort(front_arr) + quick_sort(pivot_arr) + quick_sort(rear_arr)

~~~

translate: Find just one ORF (because the anwser is only one) and translate it<br>

quickSort: When removing introns, it is possible to use linear search if introns are sorted by its start index<br>

-----

~~~python
name = input()
f = open(name, 'r')
f.readline()
count = 0
seq = ""
reExp = []
input_intron = ""

for line in f.readlines():
    if line[0] != '>':
        if count == 0:
            seq += line[:-1]
        else:
            input_intron += line[:-1]

    else:
        if count == 0:
            count += 1
        else:
            reExp.append(input_intron)
            input_intron = ""

reExp.append(input_intron)

~~~

Extract a given DNA sequence (seq) and several introns (reExp)<br>

-----

~~~python
candidate_seqs = []
ori_intron_location = []
overlap_intron_location = []

class Intron:
    def __init__(self, x, y):
        self.__x = x
        self.__y = y
        self.__pair = []

    def getX(self):
        return self.__x
    def getY(self):
        return self.__y
    def getPair(self):
        return self.__pair
    def appendPair(self, other):
        self.__pair.append(other)
    def __eq__(self, other):
        return self.__x == other.__x and self.__y == other.__y

~~~

Intron class: appendPair (overlap_intron has pair instance variable and it will have its overlapped ori_intron)<br>

candidate_seqs: DNA Sequences whose introns are removed<br>

ori_intron_location: The location of intron sequences that were extracted through regular expression without "Lookahead Assertion"<br>

overlap_intron_location: The location of intron sequences that were extracted through "Lookahead Assertion" (but, intron which has already been involved in ori_intron_location shouldn't be involved in) + ori_intron sequences that are ovelpped with other ori_intron (These will be removed from ori_intron_location and move to overlap_intron_location)<br>

~~~
Example

Intron: ACCT, CTTA, GCGC

DNA sequence: ATGACCTTACAGCGCGCATTAG
                 ----
                   ----  ----
                           ----

RE (without lookahead assertion): 3 (RE: ACCT), 5 (RE: CTTA), 11 (RE: GCGC) -> ori_intron_location

RE (with lookahead assertion): 3 (RE: ACCT), 5 (RE: CTTA), 11 (RE: GCGC), 13 (RE: GCGC) -> Only 13 is the element of overlap_intron_location

In ori_intron_location, 5 (RE: CTTA) is overlapped with 3 (RE: ACCT)
-> 5 (RE: CTTA) is removed from ori_intron and move to overlap_intron
~~~

-----

~~~python
for i in range(len(reExp)):
    oneReExp = reExp[i]
    compiler = re.compile(oneReExp)
    introns = compiler.finditer(seq)

    for intron in introns:
        x = intron.span()[0]
        y = intron.span()[1]

        newIntron = Intron(x, y)
        ori_intron_location.append(newIntron)

for i in range(len(reExp)):
    oneReExp = "(?=" + reExp[i] + ")"
    compiler2 = re.compile(oneReExp)
    introns = compiler2.finditer(seq)

    for intron in introns:
        x = intron.span()[0]
        y = x + len(reExp[i])


        if Intron(x, y) in ori_intron_location:
            continue
        else:
            newIntron = Intron(x, y)
            overlap_intron_location.append(newIntron)

~~~

RE (without lookahead assertion) -> ori_intron_location<br>

RE (with lookahead assertion) + remove duplicated intron within ori_intron_location -> overlap_intron_location<br>

-----

~~~python
removedOriIntron = []
for i in range(len(ori_intron_location)):
    ori_intron1 = ori_intron_location[i]
    for j in range(i, len(ori_intron_location)):
        if i == j:
            continue
        else:
            ori_intron2 = ori_intron_location[j]
            if (ori_intron2.getX() >= ori_intron1.getX() and ori_intron2.getX() <= ori_intron1.getY() - 1) or (ori_intron2.getY() >= ori_intron1.getX() and ori_intron2.getY() <= ori_intron1.getY() - 1):
                removedOriIntron.append(ori_intron2)
                overlap_intron_location.append(ori_intron2)

new_ori_intron_location = []               
for i in range(len(ori_intron_location)):
    if ori_intron_location[i] not in removedOriIntron:
        new_ori_intron_location.append(ori_intron_location[i])

~~~

ori_intron sequences that are ovelpped with other ori_intron (These will be removed from ori_intron_location and move to overlap_intron_location)<br>

After this process, ori_intron_location -> new_ori_intron_location<br>

-----

~~~python
for i in range(len(overlap_intron_location)):
    overlap_intron = overlap_intron_location[i]
    for j in range(len(new_ori_intron_location)):
        new_ori_intron = new_ori_intron_location[j]

        if (overlap_intron.getX() >= new_ori_intron.getX() and overlap_intron.getX() <= new_ori_intron.getY() - 1) or (overlap_intron.getY() >= new_ori_intron.getX() and overlap_intron.getY() <= new_ori_intron.getY() - 1):
            overlap_intron.appendPair(new_ori_intron)

~~~

For overlap_intron, appendPair<br>

-----

~~~python
for i in range(2 ** len(overlap_intron_location)):
    possibleSeq = ""
    removed_intron = []
    pre_ori_intron = -1
    for j in range(len(overlap_intron_location)):
        if i & (1 << j) != 0:
            for k in range(len(overlap_intron_location[j].getPair())):
                removed_intron.append(overlap_intron_location[j].getPair()[k])
        else:
            removed_intron.append(overlap_intron_location[j])

~~~

Use Brute-force algorithm (exhaustive search, 완전탐색) through bitmask<br>

~~~
(overlap_intron) intron 1    intron 2    intron 3

i = 0               x           x           x     bit: 000 = 0 -> seq1
i = 1               o           x           x     bit: 100 = 1 -> seq2
i = 2               x           o           x     bit: 010 = 2 -> seq3
i = 3               o           o           x     bit: 110 = 3 -> seq4
i = 4               x           x           o     bit: 001 = 4 -> seq5
i = 5               o           x           o     bit: 101 = 5 -> seq6
i = 6               x           o           o     bit: 011 = 6 -> seq7
i = 7               o           o           o     bit: 111 = 7 -> seq8

possibleSeq: seq1 ~ seq8
~~~

removed_intron: its element is the ori_intron & overlap_intron which are determined to remove<br>

pre_ori_intron: When slicing the given DNA sequence for getting mature mRNA sequence, we need the y (last index) value of previous ori_intron<br>

-----

~~~python
for i in range(2 ** len(overlap_intron_location)):
    ///
    new_ori_intron_location = new_ori_intron_location + overlap_intron_location
    new_ori_intron_location = quick_sort(new_ori_intron_location)          

    for l in range(len(new_ori_intron_location)):
        if new_ori_intron_location[l] in removed_intron:
            continue
        else:
            if pre_ori_intron != -1:
                possibleSeq += seq[pre_ori_intron : new_ori_intron_location[l].getX()]


            else:
                possibleSeq += seq[: new_ori_intron_location[l].getX()]


            pre_ori_intron = new_ori_intron_location[l].getY()

    possibleSeq += seq[pre_ori_intron :]

    candidate_seqs.append(possibleSeq)


~~~

Doing "for" loop for (new_ori_intron_location + overlap_intron_location) after doing quickSort<br>

If intron is involved in removed_intron, continue<br>

pre_ori_intron == -1 means that it is the first intron<br>

**possibleSeq = seq[:intron 1's x value] + seq[intron 1's y value : intron 2's x value] + seq[intron 2's y value : intron 3's x value] + .... + seq[last intron's y value :]**<br>

-----

~~~python
proteins = []
for i in range(len(candidate_seqs)):
    proteins.append(translate(candidate_seqs[i]))

for i in range(len(proteins)):
    print(proteins[i])

f.close()
~~~
