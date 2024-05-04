---
layout: post
title: Sieve of Eratosthenes
description: >
  Algorithm
tags: [sieve of Eratosthenes]
use_math: true
categories:
  - study
  - computer science
---
### Sieve of Eratosthenes
* 에라토스테네스의 체
* Given 1, 2, 3, ..., N, it can be used for calculating the number of prime numbers

### Primality Test
* Given one number (N), how can we determine whether it is prime number or not?
* Brute-force Algorithm : O(N)

<br>

~~~python
N = int(input())
flag = True

for i in range(2, N):
  if N % i == 0:
    flag = False

print(flag)
~~~

* We can save the time of that algorithm : **O(√N)**
  * Reason
    * Given integer A, B (A x B = N)
    * If 1 <= A <= int(√N), **int(√N) + 1 <= B <= N** (If √N is integer, A == B)
    * Consequently, it can be determined even if we just search it from 2 to √N

<br>

~~~python
N = int(input())
flag = True

for i in range(2, int(N ** 0.5)):
  if N % i == 0:
    flag = False

print(flag)
~~~

* Example Problem1
This is the private repository where I can only review my notes<br>
[Repository](https://github.com/hyun-jin891/hidden-post-hyunjin891-github-blog/blob/master/_posts/study/computer%20science/2024-02-11-Sieve-of-Eratosthenes.md)

### Use Sieve of Eratosthenes
* Given 1, 2, 3, ..., N → We should find the number of prime numbers
* While searching all of numbers (1, ..., N), we apply primality test to target number
  * O(N * √N) = O(N<sup>1.5</sup>)
* We can save the time if we use sieve of Eratosthenes (O(NloglogN))
  * 1<sup>st</sup> : 2 (prime number) is marked as True and the number that is 2 x integer is marked as False (The number of searching == int(N/2))
  * 2<sup>nd</sup> : 3 (prime number) is marked as True and the number that is 3 x integer is marked as False (The number of searching == int(N/3))
  * 3<sup>rd</sup> : 5 (prime number) is marked as True and the number that is 5 x integer is marked as False (The number of searching == int(N/5))
  * 4<sup>th</sup> : That is, unmarked number (2 ~ int(√N)) gonna be marked as True and this number x integer gonna be marked as False
  * Why do we search it from 2 to int(√N)?
    * The number that is not prime number has factors which are involved in 1 ~ int(√N)
    * Consequently, all of that numbers are filtered by the procedure (2 ~ √N) x integer)
  * Total number of searching = N/2 + N/3 + N/5 + ... < **N + N/2 + N/3 + N/4 + N/5 + N/6 +... + 1 ≈ NlnN**
    * If we use prime number theorem (The ratio of prime number in 1, 2, ... N is 1/lnN) → Sieve of Eratosthenes' time complexity : O(NloglogN)

* Code

~~~python
N = int(input())
prime = [True] * (N + 1)
prime[0] = False
prime[1] = False

for i in range(2, int(N ** 0.5) + 1):
    if prime[i]:
        for j in range(2 * i, N + 1, i):
            prime[j] = False

for i in range(2, N + 1):
    if prime[i]:
        print(i)
~~~

* Example Problem2
This is the private repository where I can only review my notes<br>
[Repository](https://github.com/hyun-jin891/hidden-post-hyunjin891-github-blog/blob/master/_posts/study/computer%20science/2024-02-11-Sieve-of-Eratosthenes.md)
