---
layout: post
title: Locating Restriction Sites
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, String Algorithm
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Locating Restriction Sites
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/revp/)

* Description
  * Palindrome Sequence (= Reverse Palindrome)
    * The sequence that is same with its reverse complement

    ~~~
        5-GATATC-3
        3-CTATAG-5 -> 5-GATATC-3 (Reverse Complement)

    ~~~
  * Restriction Enzymes
    * Most of them are homodimer
    * Each monomer recognizes the **identical** sequence pattern (4~12 nt) within **one strand** (not double strands)
    * **Way for cutting both strands**
      * Case 1 (non-palindrome sequence)
        * There are a few restriction enzymes which recognize the non-palindrome sequence
        * **Condition**: The strand which has a sequence pattern also has its reverse complement
          * **The closer its reverse complement is to the sequence pattern, the greater probability of cutting both strands is** (Distance barrier between each monomer of restriction enzymes)

        ~~~
         Sequence Pattern: 5-CㅣAAGGT-3 (non-palindrome sequence)

          5-.....CㅣAAGGTACCTT  G....-3    One Strand
          3-.....G  TTCCATGGAAㅣC....-5    The Other Strand

          5-C             AAGGTACCTTG-3
          3-GTTCCATGGAA             C-5       Sticky Ends
        ~~~

      * Case 2 (palindrome sequence)
        * Most of the restriction enzymes recognize the palindrome sequence

        ~~~
        Sequence Pattern: 5-GAAㅣTTC-3 (palindrome sequence)

          5-.....GAAㅣTTC....-3    One Strand
          3-.....CTTㅣAAG....-5    The Other Strand

          5-GAA    TTC-3
          3-CTT    AAG-5       Blunt Ends  
        ~~~

    * Sticky Ends vs Blunt Ends
      * If the restriction enzymes cut the middle point within the sequence pattern -> Blunt Ends
      * Otherwise -> Sticky Ends
  * The rosalind problem asks us to calculate the location and length of palindrome sequences within a given DNA sequence    

### My Solution
* Sketch
  * We need to find the palindrome sequence that is recognized by the restriction enzymes because the condition of palindrome sequence is its length is 4~12
  * The length of palindrome sequence need to be even number (4, 6, 8, 10, 12)
    * For 4, 6, 8, 10, 12 length of subsequence, I do linear search and check whether the subsequence is palindrome sequence or not

* Code

<br>

~~~python
res = []


def complement(base):
    if base == "A":
        return "T"
    elif base == "T":
        return "A"
    elif base == "G":
        return "C"
    else:
        return "G"


def isPalindrome(seq):
    for i in range(len(seq) // 2):
        if seq[i] != complement(seq[len(seq) - 1 - i]):
            return False

    return True

def reversePalindrome(seq):
    for i in range(4, 13, 2):
        for j in range(len(seq) - i + 1):
            subseq = seq[j : j + i]
            if (isPalindrome(subseq)):
                res.append((j, len(subseq)))


name = input()
f = open(name, 'r')

DNA_seq = ""
f.readline()

for line in f.readlines():
    DNA_seq += line[:-1]


reversePalindrome(DNA_seq)

for p, l in res:
    print(p + 1, l)

f.close()
~~~
