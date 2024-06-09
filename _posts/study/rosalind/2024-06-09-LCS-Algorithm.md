---
layout: post
title: LCS Algorithm
description: >
  For finding the LCS from 2 DNA sequences
tags: [Motifs]

categories:
  - study
  - rosalind
---

### LCS Algorithm
* LCS has 2 meanings
  * The Longest Common Substring
    * From 2 strings, we extract the longest common substring (need to be continuous)
  * The Longest Common Subsequence
    * From 2 strings, we extract the longest common subsequence (don't need to be continuous)
* Using LCS Algorithm -> we can guess the longest candidate motif between 2 sequences
  * substring : not considering the mutation
  * subsequence : considering the mutation

### Longest Common Substring
* Sketch
  * Use dynamic programming with 2D array
  * Rows (seq1[i]) & Columns (seq2[j]) consist of two given sequences
  * seq1[i] == seq2[j]
    * dp[i][j] = dp[i - 1][j - 1] + 1
    * Only seq1 == seq2 location has the value (> 0)
    * If a diagonal path on dp consists of the sequence like 1, 2, 3, ..
      * It is the common substring
    * Consequently, maximum value of dp is the end of the longest common substring (Through backtracking, we can find the whole lcs)
  * seq1[i] != seq2[j]
    * dp[i][j] = 0
  * i == 0 or j == 0 (sequence starts from index 1)
    * dp[i][j] = 0
* Code

<br>

~~~python
seq1 = input()
seq2 = input()

dp = [[0 for i in range(len(seq2) + 1)] for i in range(len(seq1) + 1)]

maxValue = 0
maxValue_y, maxValue_x = 0, 0

for i in range(1, len(seq1) + 1):
  for j in range(1, len(seq2) + 1):
    if seq1[i - 1] == seq2[j - 1]:
      dp[i][j] = dp[i - 1][j - 1] + 1
    else:
      dp[i][j] = 0

    if maxValue < dp[i][j]:
      maxValue = dp[i][j]
      maxValue_x = j
      maxValue_y = i

LCS = ""

for i in range(maxValue):
  LCS += seq1[maxValue_y - (maxValue - 1 - i) - 1]

print(LCS)
~~~


### Longest Common Subsequence
* Sketch
  * It is similar with the longest common substring except that it doesn't need to be continuous
  * Use dynamic programming with 2D array
  * Rows (seq1[i]) & Columns (seq2[j]) consist of two given sequences
  * seq1[i] == seq2[j]
    * dp[i][j] = dp[i - 1][j - 1] + 1
    * Only seq1 == seq2 location increases the value of dp
  * seq1[i] != seq2[j]
    * dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    * This location of dp cannot be the element of the lcs
    * We have to take the value from the previous location directly
  * i == 0 or j == 0 (sequence starts from index 1)
    * dp[i][j] = 0
  * Finding LCS
    * Start from dp[last index][last index] -> moving index
    * dp[i][j]
      * If either dp[i - 1][j] or dp[i][j - 1] is equal to dp[i][j], we move to either of them (Consequently, lcs can be multiple)
      * If neither dp[i - 1][j] nor dp[i][j - 1] is equal to dp[i][j], we move to dp[i - 1][j - 1]
        * The character of the location (i, j) has to get into the LCS (reverse)
      * If dp[i][j] == 0, terminate
* Code

<br>

~~~python
seq1 = input()
seq2 = input()

dp = [[0 for i in range(len(seq2) + 1)] for i in range(len(seq1) + 1)]

maxValue = 0
maxValue_y, maxValue_x = 0, 0

for i in range(1, len(seq1) + 1):
  for j in range(1, len(seq2) + 1):
    if seq1[i - 1] == seq2[j - 1]:
      dp[i][j] = dp[i - 1][j - 1] + 1
    else:
      dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

LCS = ""

i = len(seq1)
j = len(seq2)


while dp[i][j] != 0:
  if dp[i - 1][j] == dp[i][j] or dp[i][j - 1] == dp[i][j]:
    if dp[i - 1][j] == dp[i][j]:
      i = i - 1
    else:
      j = j - 1
  else:
    LCS += seq1[i - 1]       # len(dp) > len(seq)
    i = i - 1
    j = j - 1

for i in range(len(LCS)):
    print(LCS[len(LCS) - 1 - i], end='')
~~~
