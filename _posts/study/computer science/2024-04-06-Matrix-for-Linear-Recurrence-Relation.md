---
layout: post
title: Matrix for Linear Recurrence Relation
description: >
  Algorithm
tags: [matrix]
use_math: true
categories:
  - study
  - computer science
---
### Matrix for Linear Recurrence Relation
* Linear Recurrence Relation (선형 점화식)
  * a<sub>n</sub> = Sum of (constant x previous a)
  * ex) Fibonacci : a<sub>n</sub> = a<sub>n-1</sub> + a<sub>n-2</sub>
* It can work out through DP
* It can work out through **matrix** (행렬) more effectively than DP (O(logN))

### Fibonacci
* a<sub>n</sub> = a<sub>n-1</sub> + a<sub>n-2</sub>
* Thinking

<br>

~~~
           1  1
  Let A =     
           1  0

  a3                a2
         =   A   x            (a3 = a1 + a2)
  a2                a1


  a4                a3                a2
         =   A   x         =  A**2 x  
  a3                a2                a1

                   .
                   .
                   .
                   .
                   .

  a(n+1)               a2
         = A**(n-1) x   
  an                   a1

                               b   c
  ∴ If calculated A**(n-1) =
                               d   e

     an = d x a2 + e x a1

~~~

<br>

* Code

<br>


~~~python
n = int(input())

def multiply(A, B):
    res = [[0 for i in range(2)] for i in range(2)]

    for i in range(2):
        for j in range(2):
            for k in range(2):
                res[i][j] += (A[i][k] * B[k][j])

    return res

def modpow(A, r):
    res = A
    intermediate = A
    flag = True

    for i in range(60):
        if r < (1 << i):
            break
        if r & (1 << i) != 0:
            if flag:
                res = intermediate
                flag = False
            else:
                res = multiply(res, intermediate)

        intermediate = multiply(intermediate, intermediate)

    return res

A = [[1, 1], [1, 0]]
mod = 10 ** 9

res = modpow(A, n - 1)

print(res[1][0] + res[1][1])

~~~


<br>

* multiply : multiplication of matrix
* modpow : iterative square method for matrix

### Other Example
* a<sub>n</sub> = 2a<sub>n-1</sub> + a<sub>n-2</sub>

<br>

~~~
                    2  1
          Let A  =
                    1  0

         a3                a2
                =   A   x            (a3 = a1 + 2 x a2)
         a2                a1


         a4                a3                a2
                =   A   x         =  A**2 x  
         a3                a2                a1

                  .
                  .
                  .

~~~

<br>

* a<sub>n</sub> = a<sub>n-1</sub> + a<sub>n-2</sub> + a<sub>n-3</sub>

<br>

~~~
                    1  1  1
          Let A  =  1  0  0
                    0  1  0


         a4                 a3
         a3      =   A   x  a2       (a4 = a1 + a2 + a3)
         a2                 a1


         a5                 a4               a3
         a4      =   A   x  a3   =  A**2  x  a2
         a3                 a2               a1

                  .
                  .
                  .

~~~

<br>

### Problem
* book : 문제 해결을 위한 알고리즘 with 수학, p.287 문제 4.7.4

<br>


~~~
4 x N 크기의 직사각형을 1x2 또는 2x1 크기의 직사각형으로 완전히 채우는 방법의 수를 1000000007로 나눈 나머지를 구하는 프로그램을 작성해주세요.
~~~


<br>

* Sketch

  ![그림1](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/163.png?raw=true){: width="400" height="800"}<br>

  * We should let initial state be 16<sup>th</sup> state

* Code

<br>


~~~python
n = int(input())
mod = 1000000007

def multiply(A, B, mod):
    res = [[0 for i in range(16)] for i in range(16)]

    for i in range(16):
        for j in range(16):
            for k in range(16):
                res[i][j] += (A[i][k] * B[k][j]) % mod

    return res

def modpow(A, r, mod):
    res = A
    intermediate = A
    flag = True

    for i in range(60):
        if r < (1 << i):
            break
        if r & (1 << i) != 0:
            if flag:
                res = intermediate
                flag = False
            else:
                res = multiply(res, intermediate, mod)

        intermediate = multiply(intermediate, intermediate, mod)

    return res

A = [[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1], [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1], [0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1], [0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0], [0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0], [0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0], [0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0], [1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 0, 1]]

res = modpow(A, n, mod)

print(res[15][15])

~~~



<br>
