---
layout: post
title: Calculating Protein Mass
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Mathematical Periods, Python Module
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Calculating Protein Mass
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/prtm/)

* Description
  * Mass Spectrometry
    * "an analytical chemical technique used to determine the mass, elemental composition, and structure of molecules"
    * Monoisotopic Mass
      * There are various isotopic substance (Even if they are same substance, their mass is different from each other)
      * If we calculate total mass through monoisotopic mass, we need to use the most abundant isotope's mass
    * Average Mass
      * If we calculate total mass through average mass, we need to use the average mass of atoms
  * Protein Mass = Mass(residue 1) + Mass(residue 2) + ... + Mass(residue n) - (n - 1) x Mass(H<sub>2</sub>O)
    * Because of Dehydration reaction
    * In several experiment, we often ignore non-residue's mass (water) when we calculate the protein mass
      * This is a safe assumption because we often do **Tandem Mass Spectrometry**
      * Tandem Mass Spectrometry: "protein is first divided into peptides, which are then broken into ions for mass analysis"
  * There is a Monoisotopic Mass Table for each amino acid

### My Solution

* Code

<br>

~~~python
mass_table = {"A" : 71.03711, "C" : 103.00919, "D" : 115.02694, "E"  : 129.04259, "F"  : 147.06841, "G"  : 57.02146, "H"  : 137.05891, "I"  : 113.08406, "K"  : 128.09496, "L"  : 113.08406, "M"  : 131.04049, "N"  : 114.04293, "P"  : 97.05276, "Q"  : 128.05858, "R"  : 156.10111, "S"  : 87.03203, "T"  : 101.04768, "V"  : 99.06841, "W"  : 186.07931, "Y"  : 163.06333}

name = input()
f = open(name, 'r')
aaSeq = ""

for line in f.readlines():
    aaSeq += line[:-1]


mass = 0

for i in range(len(aaSeq)):
    mass += mass_table[aaSeq[i]]

print(mass)
f.close()
~~~
