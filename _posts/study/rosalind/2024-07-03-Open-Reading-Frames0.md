---
layout: post
title: Enumerating Gene Orders
description: >
  A problem from rosalind "Bioinformatics Stronghold" category, Mathematical Periods, Python Module
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Enumerating Gene Orders
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/perm/)

* Description
  * In the biology, we sometimes assign the positive integer number to the specific unit (like a specific gene, chromosome regions, and so on) for identifying each other and organizing (like sorting)
  * When we want to organize these, we can use "**Permutation** (순열)"
  * This rosalind problem asks us to calculate permutation value and all the cases representing the way organizing data

### My Solution
* Sketch
  * I make the function for calculating permutation through the recursion (Factorial)
  * I use "Mathematical Periods" for getting all the cases representing the way organizing data
    * I found the period when we write these cases

    ~~~
     Result (n = 4): We organize "1234"
     Permutaion: 4! = 24
     Each "" will have 4 characters (like 1234 or 4321)

     ""   ""   ""   ""   ""   ""
     ""   ""   ""   ""   ""   ""
     ""   ""   ""   ""   ""   ""
     ""   ""   ""   ""   ""   ""
    ~~~

    ~~~
    First character of each "" has period: 6
    It can be calculted through n! / n

    "1"   "1"   "1"   "1"   "1"   "1"
    "2"   "2"   "2"   "2"   "2"   "2"
    "3"   "3"   "3"   "3"   "3"   "3"
    "4"   "4"   "4"   "4"   "4"   "4"

    ~~~

    ~~~
    Second character of each "" has period: 2
    It can be calculted through (n - 1)! / (n - 1)
    The pool of number being able to be involved in "" goes through the initialization per n! / n

    "12"   "12"   "13"   "13"   "14"   "14"   Poor: [2, 3, 4]
    "21"   "21"   "23"   "23"   "24"   "24"   Poor: [1, 3, 4]
    "31"   "31"   "32"   "32"   "34"   "34"   Poor: [1, 2, 4]
    "41"   "41"   "42"   "42"   "43"   "43"   Poor: [1, 2, 3]
    ~~~

    ~~~
    Third character of each "" has period: 1
    It can be calculted through (n - 2)! / (n - 2)
    The pool of number being able to be involved in "" goes through the initialization per (n-1)! / (n-1)

    "123"   "124"   "132"   "134"   "142"   "143"   
    "213"   "214"   "231"   "234"   "241"   "243"   
    "312"   "314"   "321"   "324"   "341"   "342"   
    "412"   "413"   "421"   "423"   "431"   "432"

                        ↓
    "123"   "124"    Poor: [3, 4]
    "132"   "134"    Poor: [2, 4]
    "142"   "143"    Poor: [2, 3]
    "213"   "214"    Poor: [3, 4]
    "231"   "234"    Poor: [1, 4]
    "241"   "243"    Poor: [1, 3]
    "312"   "314"    Poor: [2, 4]
    "321"   "324"    Poor: [1, 4]
    "341"   "342"    Poor: [1, 2]
    "412"   "413"    Poor: [2, 3]
    "421"   "423"    Poor: [1, 3]
    "431"   "432"    Poor: [1, 2]
    ~~~

    ~~~
    Fourth character of each "" has period: 1
    It can be calculted through (n - 3)! / (n - 3)
    The pool of number being able to be involved in "" goes through the initialization per (n-2)! / (n-2)

    "1234"   Poor: [4]
    "1243"   Poor: [3]
    "1324"   Poor: [4]
    "1342"   Poor: [2]
    "1423"   Poor: [3]
    "1432"   Poor: [2]
    "2134"   Poor: [4]
    "2143"   Poor: [3]
    "2314"   Poor: [4]
    "2341"   Poor: [1]
    "2413"   Poor: [3]
    "2431"   Poor: [1]
    "3124"   Poor: [4]
    "3142"   Poor: [2]
    "3214"   Poor: [4]
    "3241"   Poor: [1]
    "3412"   Poor: [2]
    "3421"   Poor: [1]
    "4123"   Poor: [3]
    "4132"   Poor: [2]
    "4213"   Poor: [3]
    "4231"   Poor: [1]
    "4312"   Poor: [2]
    "4321"   Poor: [1]
    ~~~

    ~~~
    ith character of each "" has period: (n - (i - 1))! / (n - (i - 1))
    The pool of number being able to be involved in "" goes through the initialization per (n - (i - 1) - 1)! / (n - (i - 1) - 1) (That is, it is previous period)

    "1234"   Poor: [4]
    "1243"   Poor: [3]
    "1324"   Poor: [4]
    "1342"   Poor: [2]
    "1423"   Poor: [3]
    "1432"   Poor: [2]
    "2134"   Poor: [4]
    "2143"   Poor: [3]
    "2314"   Poor: [4]
    "2341"   Poor: [1]
    "2413"   Poor: [3]
    "2431"   Poor: [1]
    "3124"   Poor: [4]
    "3142"   Poor: [2]
    "3214"   Poor: [4]
    "3241"   Poor: [1]
    "3412"   Poor: [2]
    "3421"   Poor: [1]
    "4123"   Poor: [3]
    "4132"   Poor: [2]
    "4213"   Poor: [3]
    "4231"   Poor: [1]
    "4312"   Poor: [2]
    "4321"   Poor: [1]
    ~~~

* Code

<br>

~~~python
def factorial(n):
    if n == 1 or n == 0:
        return 1
    return n * factorial(n - 1)


name = input()
f = open(name, "r")

n = f.readline()
n = int(n)

count = 0
numList = list(range(1, n + 1))

permutation = factorial(n)

res = ["" for i in range(permutation)]


tryNum = 0
preElementNumber = 0

while tryNum < n:
    tryNum += 1

    elementNumber = factorial(n - (tryNum - 1)) / (n - tryNum + 1)
    alreadyInvolveNum = []
    temp = 0
    for i in range(len(res)):
        if preElementNumber != 0 and i % preElementNumber == 0:
            alreadyInvolveNum = []
        else:
            if i % elementNumber != 0:
                temp = 0
            elif i != 0:
                alreadyInvolveNum.append(temp)
        for j in range(len(numList)):      
            candidateNum = numList[j]
            if candidateNum in alreadyInvolveNum:
                continue
            if str(candidateNum) not in res[i]:
                res[i] += str(candidateNum) + " "
                temp = candidateNum
                break
    preElementNumber = elementNumber

print(permutation)
for i in range(len(res)):
    print(res[i])

f.close()
~~~

## Simple Answer
* There have already been tools for calculating permutation and all the cases representing the way organizing data
* Calculating permutation

~~~python
from math import factorial
number_of_permutations = factorial(n)
~~~

* All the cases representing the way organizing data

~~~python
from itertools import permutations
list_of_permutations = list(permutations(range(1,n+1)))

~~~
