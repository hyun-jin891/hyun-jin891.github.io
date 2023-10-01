---
layout: post
title: Identify Deterministic Motif
description: >
  For identifying motif from the suggested sequences
tags: [Motifs]

categories:
  - study
  - rosalind
---

### Identify Deterministic Motif
* There are some sequences which have the common motif
* However, we don't know what the motif is (we know the size)
* We should identify the indexes representing the start point of motif in each suggested sequence (It gonna be saved as index_list)
* After defining the score representing how much the list is correct, we calculate and select the maximum score<br>

![그림1](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/147.PNG?raw=true){: width="600" height="600"}<br>



~~~python
def frequencyMatrix(index_list, motif_size, seq_list, alphabet_list = ['A', 'G', 'T', 'C']):
  result = [[0 for i in range(motif_size)] for i in range(4)]

  for i in range(len(index_list)):
    subseq = seq_list[i][index_list[i] : index_list[i] + motif_size]
    for j in range(motif_size):    
      for k in range(len(alphabet_list)):
        if subseq[j] == alphabet_list[k]:
          result[k][j] += 1

  return result

def score(index_list, motif_size, seq_list):
  matrix = frequencyMatrix(index_list, motif_size, seq_list)
  score = 0
  maxFrequency = 0

  for i in range(motif_size):
    for j in range(4):
      if maxFrequency < matrix[j][i]:
        maxFrequency = matrix[j][i]
    score += maxFrequency
    maxFrequency = 0

  return score
~~~

### Brute Force Algorithm
* Search all cases
  * index_list = [0, 0, 0, ... , 0] starts
  * [0, 0, ... , 0, 0] → [0, 0, ... , 0, 1] → [0, 0, ... , 0, 2] → ... → [0, 0, ... , 0, seq_size - motif_size] → [0, 0, ... , 1, 0] → ... → [0, 0, ... , 1, seq_size - motif_size] → .....
  * Choose maximum score
* Accurate but it is not efficient

<br>

~~~python
def nextCase(index_list, seq_list, motif_size):
  result = [0 for i in range(len(index_list))]
  pos = len(index_list) - 1

  while pos >= 0 and index_list[pos] == len(seq_list[pos]) - motif_size:
    pos -= 1

  if pos < 0:
    return None

  for i in range(pos):
    result[i] = index_list[i]

  result[pos] = index_list[pos] + 1

  for i in range(pos + 1, len(seq_list)):
    result[i] = 0

  return result

def BruteForce(seq_list, motif_size):
  result = []
  index_list = [0 for i in range(len(seq_list))]
  maxScore = -1

  while index_list != None:
    currentScore = score(index_list, motif_size, seq_list)
    if currentScore > maxScore:
      maxScore = currentScore
      result = index_list

    index_list = nextCase(index_list, seq_list, motif_size)

  return result
~~~

<br>

* nextCase : Change the index_list which gonna be inspected
  * Start from the last element of index_list
  * If target reaches seq_size - motif_size, target gonna be moved to current index - 1 (current index ~ end : start from 0)
* BruteForce : find the index_list having maximum score

### Bypass
* remove some cases from brute force algorithm
* When we focus on the target (certain location (i) of index_list whose a value gonna be changed : i + 1 ~ end already removed in index_list because we are not interested in them)
  * If **estimated** optimum score considering (i + 1 ~ end) is less than current maximum score, we won't search it (bypass)
    * optimum score = score(start ~ i) + (end - i) * motif_size
    * In short, just index_list[i] += 1
  * If not, we should search it
    * While index_list[i]'s value remains, we add 0 to index_list[i + 1] for searching
* It is more efficient than Brute Force Algorithm, but it is still inefficient

<br>

~~~python
def UseByPass(seq_list, motif_size):
  index_list = [0 for i in range(len(seq_list))]
  result = []
  maxScore = -1

  while index_list != None:
    if len(index_list) < len(seq_list):
      optimum_score = score(index_list, motif_size, seq_list) + (len(seq_list) - len(index_list)) * motif_size
      if optimum_score < maxScore:
        index_list = byPass(index_list, motif_size, seq_list)
      else:
        index_list = noPass(index_list, motif_size, seq_list)
    else:
      currentScore = score(index_list, motif_size, seq_list)
      if currentScore > maxScore:
        maxScore = currentScore
        result = index_list
      index_list = noPass(index_list, motif_size, seq_list)

  return result

def noPass(index_list, motif_size, seq_list):
  result = []
  if len(index_list) < len(seq_list):
    for i in range(len(index_list)):
      result.append(index_list[i])
    result.append(0)
  else:
    pos = len(index_list) - 1

    while pos >= 0 and index_list[pos] == len(seq_list[pos]) - motif_size:
      pos -= 1

    if pos < 0:
      return None

    for i in range(pos):
      result.append(index_list[i])
    result.append(index_list[pos] + 1)

  return result

def byPass(index_list, motif_size, seq_list):
  pos = len(index_list) - 1
  result = []

  while pos >= 0 and index_list[pos] == len(seq_list[pos]) - motif_size:
    pos -= 1

  if pos < 0:
    return None

  for i in range(pos):
    result.append(index_list[i])
  result.append(index_list[pos] + 1)

  return result
~~~

<br>

### Heuristic Algorithm
* Both brute Force Algorithm and bypass method are not efficient when size of data is big
* We just gonna use reasonable logic for quickly solving it even if it can yield somewhat inaccurate result (Heuristic Algorithm)
* Logic
  * We just calculate the maximum score only between first element and second element from index_list
  * After that, we focus on 3 ~ end **sequentially** and update the maximum score
* We should repeat the calculation with changing both first element and second element for the accurate result

<br>

~~~python
def Heuristic(motif_size, seq_list):
  result = [0 for i in range(len(seq_list))]
  tempoRes = [0, 0]
  maxScore = -1

  for i in range(len(seq_list[0]) - motif_size):
    for j in range(len(seq_list[1]) - motif_size):
      tempoRes[0] = i
      tempoRes[1] = j
      currentScore = score(tempoRes, motif_size, seq_list)

      if currentScore > maxScore:
        maxScore = currentScore
        result[0] = i
        result[1] = j

  for i in range(2, len(seq_list)):
    tempoRes = [0] * (i + 1)
    for j in range(len(tempoRes)):
      tempoRes[j] = result[j]

    maxScore = -1

    for k in range(len(seq_list[i]) - motif_size):
      tempoRes[i] = k
      currentScore = score(tempoRes, motif_size, seq_list)

      if currentScore > maxScore:
        maxScore = currentScore
        result[i] = k

  return result
~~~
