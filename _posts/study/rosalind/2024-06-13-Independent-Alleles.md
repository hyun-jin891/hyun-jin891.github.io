---
layout: post
title: Independent Alleles
description: >
  A problem from rosalind "Bioinformatics Stronghold" category
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Independent Alleles
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/lia/)

* Description
  * 0<sup>th</sup> generation: AaBb
  * 1<sup>st</sup> generation: AaBb (0<sup>th</sup> generation) x AaBb
  * n<sup>th</sup> generation: ? ((n-1)<sup>th</sup> generation) x AaBb
  * Each individual gets 2 offsprings
  * Goal: The probability that at least N Aa Bb organisms will belong to the k<sup>th</sup> generation
  * The genes satisfy the law of independent assorment

### My Solution
* Sketch
  * AaBb (0<sup>th</sup> generation) x AaBb
    * Offspring **genotype ratio**

      <br>

      ~~~
                  AA       Aa      aa
            BB     1        2       1
            Bb     2        4       2
            bb     1        2       1
      ~~~
  * Critical Point: **In all of generations, the genotype ratio is maintained**
    * That is, AaBb's ratio in all of generations is **4/16 (0.75)**
    * Reason
      * The ingredient (gamete's genotype) for making offspring's genotype in 0<sup>th</sup> generation is AA, Ab, aB, ab
      * The ratio of each ingredient is maintained because the new ingredient is supplied as the same amount with AaBb (0<sup>th</sup> generation)
      * For the same reason
        * When 0<sup>th</sup> generation's genotype is α and repeat to do cross with genotype α -> First offspring's genotype ratio is maintained in all of generations
  * "The probability that at least N Aa Bb organisms will belong to the k<sup>th</sup> generation"
    * Number of offsprings in k<sup>th</sup> generation = 2<sup>k</sup>
    * If the number of AaBb is n in k<sup>th</sup> generation
      * Probability = <sub>2<sup>k</sup></sub>C<sub>n</sub> * 0.75<sup>(2<sup>k</sup> - n)</sup> * 0.25<sup>n</sup>
    * Answer_prob = 1 - <sub>2<sup>k</sup></sub>C<sub>0</sub> * 0.75<sup>(2<sup>k</sup> - 0)</sup> * 0.25<sup>0</sup> - <sub>2<sup>k</sup></sub>C<sub>1</sub> * 0.75<sup>(2<sup>k</sup> - 1)</sup> * 0.25<sup>1</sup> - <sub>2<sup>k</sup></sub>C<sub>2</sub> * 0.75<sup>(2<sup>k</sup> - 2)</sup> * 0.25<sup>2</sup> - ..... - <sub>2<sup>k</sup></sub>C<sub>N - 1</sub> * 0.75<sup>(2<sup>k</sup> - (N - 1))</sup> * 0.25<sup>N - 1</sup>

    <br>

* Code

<br>

~~~python
def factorial(n):
    if n == 1 or n == 0:
        return 1
    return n * factorial(n - 1)

def combination(n, r):
    return factorial(n)/(factorial(r) * factorial(n - r))

name = input()
f = open(name, 'r')
k, N = list(map(int, f.readline().split()))

prob = 1
num = 2 ** k

for i in range(N):
    sub_prob = combination(num, i) * (0.75 ** (num - i)) * (0.25 ** i)
    prob -= sub_prob

print(prob)
~~~
