---
layout: post
title: Open Reading Frames
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Dictionary, Regular Expression, String
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Opening Reading Frames
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/orf/)

* Description
  * Reading Frame
    * Given a DNA sequence like ACCAGTCAAGGTCATTG (Reading Direction is rightward)
    * There are 6 reading frames
      * Case 1: ㅣACCㅣAGTㅣCAAㅣGGTㅣCATㅣTG -> TSQGH
      * Case 2: AㅣCCAㅣGTCㅣAAGㅣGTCㅣATTㅣG -> PVKVI
      * Case 3: ACㅣCAGㅣTCAㅣAGGㅣTCAㅣTTGㅣ -> QSRSL
      * Reverse Complement Seq: CAATGACCTTGACTGGT
        * Case 4: ㅣCAAㅣTGAㅣCCTㅣTGAㅣCTGㅣGT -> Q_P_L
        * Case 5: CㅣAATㅣGACㅣCTTㅣGACㅣTGGㅣT -> NDLDW
        * Case 6: CAㅣATGㅣACCㅣTTGㅣACTㅣGGT -> MTLTG
    * Open Reading Frames (ORF)
      * Of the reading frames, it starts from start codon (AUG) to stop codon (UAA, UAG, UGA)
      * There should be no stop codons between the start codon and stop codon
  * This problem asks us to get all open reading frames from a given DNA sequence
    * **T instead of U** (I had to be careful when I made a codon table)

### My Solution
* Sketch
  * otherStrand function
    * It calculates the reverse complement of a given DNA sequence
  * Translate function
    * A given DNA sequence and a single index of start codon (There will be multiple indexes for multiple start codons) are the parameters of this function
    * From the start_index, extract one codon everytime the loop is excuted -> It is translated to an amino acid through codonTable dictionary
    * codonTable doesn't involve the stop codon, so the reading frame starting from the start_index is not ORF if all codons from this reading frame are translated to an amino acid
  * For the given original strand and reverse complement strand of it
    * Using regular expression, I extract the multiple indexes representing the location of multiple start codons
    * Through "Translate" function, I check whether the reading frames from these indexes are ORF or not

* Code

<br>

~~~python
import re

codonTable = {"TTT" : "F", "TTC" : "F", "TTA" : "L", "TTG" : "L", "CTT" : "L", "CTC" : "L", "CTA" : "L", "CTG" : "L", "ATT" : "I", "ATC" : "I", "ATA" : "I", "ATG" : "M", "GTT" : "V", "GTC" : "V", "GTA" : "V", "GTG" : "V", "TCT" : "S", "TCC" : "S", "TCA" : "S", "TCG" : "S", "CCT" : "P", "CCC" : "P", "CCA" : "P", "CCG" : "P", "ACT" : "T", "ACC" : "T", "ACA" : "T", "ACG" : "T", "GCT" : "A", "GCC" : "A", "GCA" : "A", "GCG" : "A", "TAT" : "Y", "TAC" : "Y", "CAT" : "H", "CAC" : "H", "CAA" : "Q", "CAG" : "Q", "AAT" : "N", "AAC" : "N", "AAA" : "K", "AAG" : "K", "GAT" : "D", "GAC" : "D", "GAA" : "E", "GAG" : "E", "TGT" : "C", "TGC" : "C", "TGG" : "W", "CGT" : "R", "CGC" : "R", "CGA" : "R", "CGG" : "R", "AGT" : "S", "AGC" : "S", "AGA" : "R", "AGG" : "R", "GGT" : "G", "GGC" : "G", "GGA" : "G", "GGG" : "G"}

def otherStrand(seq):
    resSeq = ""
    for i in range(len(seq)):
        base = seq[i]
        if base == "A":
            resSeq += "T"
        elif base == "T":
            resSeq += "A"
        elif base == "G":
            resSeq += "C"
        else:
            resSeq += "G"

    return resSeq

def Translate(seq, start_index):
    resProtein = ""
    currentIndex = start_index
    flag = False

    while (currentIndex <= len(seq) - 3):
        current_codon = seq[currentIndex : currentIndex + 3]
        if current_codon not in codonTable.keys():
            flag = True
            break
        amino_acid = codonTable[current_codon]
        resProtein += amino_acid
        currentIndex += 3

    if (flag):
        return resProtein
    else:
        return ""


name = input()
f = open(name, 'r')

Seq = ""
f.readline()

for line in f.readlines():
    Seq += line[:-1]

reExpForStart = "(?=" + "ATG" + ")"

compilerForStart = re.compile(reExpForStart)

Seq_Starts = compilerForStart.finditer(Seq)

startCodonIndex = []

for i in Seq_Starts:
    startCodonIndex.append(i.span()[0])

res = []


for i in range(len(startCodonIndex)):
    start_index = startCodonIndex[i]
    proteinSeq = Translate(Seq, start_index)
    if (proteinSeq == ""):
        continue
    else:
        res.append(proteinSeq)


Seq = otherStrand(Seq)    
reverseSeq = Seq[-1 : : -1]
Seq_Starts = compilerForStart.finditer(reverseSeq)

startCodonIndex = []

for i in Seq_Starts:
    startCodonIndex.append(i.span()[0])

for i in range(len(startCodonIndex)):
    start_index = startCodonIndex[i]
    proteinSeq = Translate(reverseSeq, start_index)
    if (proteinSeq == ""):
        continue
    else:
        res.append(proteinSeq)

res = set(res)
for seq in res:
    print(seq)
~~~
