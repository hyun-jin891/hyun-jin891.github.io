---
layout: post
title: Remainder
description: >
  Algorithm
tags: [remainder]
use_math: true
categories:
  - study
  - computer science
---
### Remainder
* 나머지
* If A % B = C & D % B = C → **A ≡ D (mod B)**

### Remainder of Addition, Subtraction, Multiplication
* Remainder (calculated result) is not changed regardless of the timing for calculation of remainder
* That is, we have A, B, C, D, E (C is (A % E), D is (B % E))
  * A + B ≡ C + D ≡ A + D ≡ C + B (mod E)
  * A x B ≡ C x D ≡ A x D ≡ C x B (mod E)
  * A - B ≡ C - D ≡ A - D ≡ C - B (mod E)
    * (object1 - object2) should not be negative
* Ex
  * (27 x 41 x 66) % 100
    * 27 x 41 = 1107 → 1107 % 100 = **7**
    * **7** x 66 = 462 → 462 % 100 = 62
  * (18 x (27 + 41 + 66)) % 10
    * sol1) (18 % 10) x (27 % 10 + 41 % 10 + 66 % 10) = 8 x (7 + 1 + 6) = 112 (still > 10) → 112 % 10 = 2
    * so12) (18 % 10) x (134 % 10) = 8 x 4 = 32 (still > 10) → 32 % 10 = 2

### Division
* When we calculate the remainder of division, we should use other method
  * A ÷ B ≡ C (mod D) : We wanna get C (< D)
  * D should be **prime number**
* General Calculation
  * A ÷ B ≡ C (mod D) : We wanna get C (< D)
  * If A % B == 0, just calculate division at first and calculate the remainder
    * ex) If A = 8, B = 2, D = 3 → 8 ÷ 2 ≡ 4 (mod 3) → C = 4
  * Unless A % B == 0 (and then **÷ is different with normal calculation** : This is only used for proving, so A % B == 0 should be satisfied in code problem)
    * A ÷ B ≡ C (mod D) → B x C ≡ A (mod D) → find desired C
    * ex) 9 ÷ 2 ≡ C (mod 11) → 2 x C ≡ 9 (mod 11) → C = 10
    * ex) 1 ÷ 6 ≡ C (mod 11) → 6 x C ≡ 1 (mod 11) → C = 2
* Use modular inverse (모듈러 역수)
  * Definition
    * A ÷ B ≡ C (mod D) ↔ A x B<sup>-1</sup> ≡ C (mod D) : B<sup>-1</sup> is modular inverse of B
    * We can find modular inverse if we use B x B<sup>-1</sup> ≡ 1 (mod D)
    * ex) 7 ÷ 3 ≡ C (mod 11)
      * 3 x B<sup>-1</sup> ≡ 1 (mod 11) → B<sup>-1</sup> = 4
      * 7 ÷ 3 ≡ C (mod 11) ↔ 7 x 4 ≡ C (mod 11) : C = 6
  * Finding modular inverse easily
    * Use Fermat's little theorem (페르마의 소정리) : B<sup>D-1</sup> ≡ 1 (mod D)
    * B x B<sup>D-2</sup> ≡ B<sup>D-1</sup> ≡ 1 ≡ B x B<sup>-1</sup>
    * B x B<sup>D-2</sup> ≡ B x B<sup>-1</sup> ≡ 1
    * B<sup>-1</sup> = **B<sup>D-2</sup>**
    * Consequently, A ÷ B ≡ C (mod D) ↔ A x B<sup>D-2</sup> ≡ C (mod D) : **C = A x B<sup>D-2</sup> (mod D)**

### Use Remainder Calculation
* Code problems sometimes ask us to calculate the remainder of the result by a specific number (ex) if the result is too big)
* If the final result is too big, it can be slow to calculate the remainder of the final result directly
* Instead, we should calculate the remainder constantly and that remainder should be used to next calculation
  * Addition, Subtraction, Multiplication : just calculate
  * Division : calculate A x B<sup>D-2</sup> (mod D)
    * for code result being same with real result, the condition that **A % B == 0** & D should be prime number should be satisfied
* Example Problem
  * book : 문제 해결을 위한 알고리즘 with 수학, p.264 예제 4.6.6

  <br>

  ~~~
  정수 N이 주어집니다.<br>
  피보나치 수열의 N번째 항 aN을 1000000007로 나눈 나머지를 구해 주세요.
  ~~~

  <br>

  ~~~python
  N = int(input())
  res = [0] * (N + 1)
  res[1] = 1
  res[2] = 1

  for i in range(3, N + 1):
    res[i] = (res[i - 1] + res[i - 2]) % 1000000007

  print(res[-1])    # faster than only calculating print(res[-1] % 1000000007)

  ~~~

### Use Iterative Square Method
* 반복제곱법
* When we calculate the remainder of division, we should calculate A x B<sup>D-2</sup> (mod D)
  * If B<sup>D-2</sup> is too big, B<sup>D-2</sup> (mod D) is hard for calculation as well
  * We can use iterative square method
* Iterative Square Method
  * ex) If we wanna calculate a<sup>25</sup> (mod D)
  * a<sup>2</sup> (mod D) = a (mod D) x a (mod D)
  * a<sup>4</sup> (mod D) = a<sup>2</sup> (mod D) x a<sup>2</sup> (mod D)
  * a<sup>8</sup> (mod D) = a<sup>4</sup> (mod D) x a<sup>4</sup> (mod D)
  * a<sup>16</sup> (mod D) = a<sup>8</sup> (mod D) x a<sup>8</sup> (mod D)
  * ....
  * a<sup>25</sup> (mod D) = a<sup>16</sup> (mod D) x a<sup>8</sup> (mod D) x a<sup>1</sup> (mod D)
    * Use binary number (25 = 2<sup>4</sup> + 2<sup>3</sup> + 2<sup>0</sup> = 11001)
    * if (b & (1 << i)) != 0:
      * i = 0 : (b & 1) != 0 → b = ?????1
      * i = 1 : (b & 10) != 0 → b = ????1?
      * i = 2 : (b & 100) != 0 → b = ???1??
      * i = 3 : (b & 1000) != 0 → b = ??1???

  <br>

  ~~~python
  def it_pow(a, b, d):
    p = a
    answer = 1

    for i in range(11):         # If b < 2048 = 2 ** 11
        if (b & (1 << i)) != 0:
          answer = (answer * p) % d
        p = (p * p) % d

    return answer

  def division(a, b, d):
    return (a * it_pow(b, d - 2, d)) % d
  ~~~

  <br>

  * In python, we just use **pow(a, b, d)** which already exists

### RSA Encryption
* book : 문제 해결을 위한 알고리즘 with 수학, p.275
* Given Sender X & Receiver Y
* Y prepares public key and private key
  * Public key : integer n & e
  * Private key : integer d which makes m<sup>ed</sup> ≡ m (mod n) be satisfied regardless of m (m < n) (m is the data from sender X)
* Y sends n & e to X
* The data which X wanna send to Y are converted to integer m
  * Receiver Y should find the value of m
* In X, calculate m<sup>e</sup> (mod n) (let the result be h)
* X sends h to Y (Y should find what m is and What Y only knows is h, n, e, d)
* m<sup>e</sup> ≡ h (mod n) → m<sup>ed</sup> ≡ h<sup>d</sup> ≡ m (mod n) because of the definition of d
* h<sup>d</sup> ≡ m (mod n) → m = h<sup>d</sup> (mod n) because of m < n
* Consequently, Y can find m
* but it is difficult for other users to find what m is because there is still no algorithm to find d only using public key

### Example Problem 1
* book : 문제 해결을 위한 알고리즘 with 수학, p.275 문제 4.6.2

~~~
2차원 격자 위의 원점 (0, 0)에 체스의 나이트가 있습니다.
나이트는 (i, j)에 있을 때, (i + 1, j + 2) 또는 (i + 2, j + 1) 중 하나로 이동
할 수 있습니다. 나이트를 (X, Y)까지 이동시키는 방법이 몇 가지 있는지 구해 주세요.
답을 1000000007로 나눈 나머지를 구하는 프로그램을 작성해주세요.
~~~

  * My solution

  <br>

  ~~~python
  X, Y = map(int, input().split())

  def division(a, b, M):
      return (a * pow(b, M - 2, M)) % M

  res = 0

  for i in range(X//2 + 1):
      if i + 2 * (X - 2 * i) < Y:
          break
        elif i + 2 * (X - 2 * i) == Y:
          a, b = 1, 1
          for j in range(2, i + X - 2 * i + 1):
              a = (a * j) % 1000000007
          for j in range(2, i + 1):
              b = (b * j) % 1000000007
          for j in range(2, X - 2 * i + 1):
              b = (b * j) % 1000000007

          res = division(a, b, 1000000007)

  print(res)
  ~~~

  <br>

  * ex) X = 5
    * case 1) ΔX : combination of +1 +1 +1 +1 +1 (+1 : 5번, +2 : 0번) → ΔY : (+1 : 0번, +2 : 5번)
    * case 2) ΔX : (+1 : 3, +2 : 1) → ΔY : (+1 : 1번, +2 : 3번)
    * ....
  * Generalization
    * Given X
      * case0) ΔY : (+1 : 0번, +2 : X번)
      * case1) ΔY : (+1 : 1번, +2 : X-2번)
      * case2) ΔY : (+1 : 2번, +2 : X-4번)
      * ....
      * caseX//2) ΔY : (+1 : **X//2번**, +2 : **2 x (X - 2 x X//2)번**)
      * If there is a case given Y in case0~X//2, we should calculate it using factorial
        * If ΔY : (+1 : 2번, +2 : X-4번)
        * Answer : (2 + X - 4)! / (2! x (X - 4)!)
  * When we calculate the factorial, it is better to use **"for" instead of recursion**
* Other solution
  * If we set the number of moving by (i + 1, j + 2) as **a** and the number of moving by (i + 2, j + 1) as **b**
  * X = a + 2b & Y = 2a + b
  * ∴ (a, b) = ((2Y-X)/3, (2X-Y)/3)
  * If 2Y-X < 0 or 2X-Y < 0 : Answer is 0
  * If (2Y-X) % 3 != 0 or (2X-Y) % 3 != 0 : Answer is 0
  * ∴ Answer = (a + b)!/(a! x b!)
