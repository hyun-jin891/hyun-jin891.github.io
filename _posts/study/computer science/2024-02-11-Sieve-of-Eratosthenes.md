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

* Example Problem
  * book : 문제 해결을 위한 알고리즘 with 수학, p.99 문제 3.1.2

<br>

  ~~~
  자연수 N을 소인수 분해하는 프로그램을 작성해 주세요.<br>

  ex) 286 = 2 x 11 x 13  >>>  answer : 2 11 13
  ~~~

  * Sketch

  ~~~
  if N % (1 ~ √N) == 0:
    N / (1 ~ √N) → repeat until N % (1 ~ √N) != 0
  ~~~

  <br>

  * Solution

  <br>

  ~~~python
  N = int(input())
  res = []

  for i in range(2, int(N ** 0.5) + 1):
      while (N % i == 0):
          res.append(i)
          N /= i

  if N >= 2:
      res.append(int(N))


  print(res)
  ~~~

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

* Example Problem
  * book : 문제 해결을 위한 알고리즘 with 수학, p.231 문제 4.4.3
  * It is important to use **N + N/2 + N/3 + ... + 1 ≈ NlnN**

  <br>

  ~~~
  양의 정수 N이 주어집니다. 정수 x의 양수인 약수 개수를 f(x)라고 할 때,<br>
  다음 값을 계산하는 프로그램을 작성해주세요. 복잡도는 O(NlogN)이 되게 해주세요.

  (1 x f(1)) + (2 x f(2)) + (3 x f(x)) + .... + (N x f(N))

  ~~~

  <br>

  * Sketch

  <br>

  ~~~
  N = 5

               1   2   3   4   5
  1 x integer  1   1   1   1   1   # If number satisfies a row, count += 1
  2 x integer  1   2   1   2   1
  3 x integer  1   2   2   2   1
  4 x integer  1   2   2   3   1
  5 x integer  1   2   2   3   2

  Consequently, 1 x 1 + 2 x 2 + 3 x 2 + 4 x 3 + 5 x 2 = 33
  O(5 + 5/2 + 5/3 + ... 1) → O(N + N/2 + N/3 + ... + 1) = O(NlogN)
  ~~~

  <br>

  * Code

    <br>

    ~~~python
    N = int(input())

    res = [1] * (N + 1)

    for i in range(2, N + 1):
        for j in range(i, N + 1, i):
            res[j] += 1

    sum = 0
    for i in range(1, N + 1):
        sum += i * res[i]

    print(sum)
    ~~~
