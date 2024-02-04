---
layout: post
title: Cumulative Sum and Difference
description: >
  Algorithm
tags: [cumulative sum and difference]
use_math: true
categories:
  - study
  - computer science
---
### Cumulative Sum and Difference
* Cumulative Sum (누적합)
  * ex. [1, 2, 3] → cumulative sum : [**1**, **3** (1 + 2), **6** (1 + 2 + 3)]
* Difference (계차)
  * L[i] - L[i-1]
  * assume 0 index : first element of original list
  * ex. [2, 8, 10, 12, 16] → difference : [**2**, **6** (8 - 2), **2** (10 - 8), **2** (12 - 10), **4** (16 - 12)]
* The **relationship** between cumulative sum and difference
  * If A list → B list by cumulative sum's procedure, A list is differece list of B list
  * If A list → B list by differece's procedure, A list is cumulative sum list of B list  

### Cumulative Sum
* If we calculate the cumulative sum list's element from the start to the last, the number of calculations is 1 + 2 + 3 + ... + N = N(N + 1)/2 → O(N<sup>2</sup>) : **slow**
* If we use difference, O(N)
  * Assume difference list : A, cumulative sum list : B
  * A<sub>i</sub> = B<sub>i</sub> - B<sub>i-1</sub> → **B<sub>i</sub> = A<sub>i</sub> + B<sub>i-1</sub>**
  * It is like Dynamic Programming

<br>

~~~python
l = [1, 2, 3]
res = [0] * 3
res[0] = l[0]

for i in range(1, len(l)):
    res[i] = res[i - 1] + l[i]

print(res)

~~~

<br>

~~~
[1, 3, 6]
~~~

### Difference
* Assume difference list : A, cumulative sum list : B
* **A<sub>i</sub> = B<sub>i</sub> - B<sub>i-1</sub>**
* O(N)

<br>

~~~python
l = [2, 8, 10, 12, 16]
res = [0] * 5
res[0] = l[0]

for i in range(1, len(l)):
    res[i] = l[i] - l[i - 1]

print(res)

~~~

<br>

~~~
[2, 6, 2, 2, 4]
~~~

### Example of Cumulative Sum
* Calculate cumulative sum list → Calculate prefix sum
* Problem (book : 문제 해결을 위한 알고리즘 with 수학, p.207 문제 4.2.1)

<br>

~~~
ALGO 철도 회사의 1호선에는 N개의 역이
있으며, 서쪽부터 차례대로 1부터 N까지의
번호가 붙어 있습니다.
역i와 역i+1은
양방향으로 연결되어 있으며,
거리가 Ai미터 입니다.
철수는 역 Bi에서 출발해서,
역 B2, B3,
 ..., BM-1을 차례대로 경유해서, 역 BM으로 가는 여행을 하려고 계획을 세웠습니다.
 철수가 여행을 하면서, 전체 몇 미터
 이동하는지 프로그램을 작성해주세요

~~~

<br>

* Input

<br>

~~~
N
A1 ... A(N-1)
M
B1
.
.
.
BM
~~~

<br>

~~~
example

4
8 6 9
6
2
1
3
2
3
4
~~~

<br>

~~~
output

43
~~~

* Solution

<br>

~~~
About input example
we can make list about the path [2, 1, 3, 2, 3, 4]
we can also make cumulative sum list of [8, 6, 9] (distance list) → [0, 8, 14, 23] (0 index : 0)

1 step : 2 → 1

Distance = cumulative_sum[1] - cumulative_sum[0] = 8


2 step : 1 → 3

Distance = cumulative_sum[2] - cumulative_sum[0] = 14

3 step : 3 → 2

Distance = cumulative_sum[2] - cumulative[1] = 6

4 step : 2 → 3

Distance = cumulative_sum[2] - cumulative_sum[1] = 6

5 step : 3 → 4

Distance = cumulative_sum[3] - cumulative_sum[2] = 9

Consequently, result is 8 + 14 + 6 + 6 + 9

43

~~~

<br>

### Example of Difference
* When all elements in specific section (ex. [1 : 4], [2 : 7], and so on) change their value with same Δ, the method slicing this section for tracking the change is slow
* We can use difference list for tracking the change quickly
  * Initialization of difference list : [0, 0, 0, 0, 0, 0 ..., 0]
  * If there gonna be +2 from index 1 to index4 in original list of difference list, the difference list is [0, 2, 0, 0, 0, -2, 0, ....., 0] (That is, difference[1] += 2 and difference[5] -= 2)
* Problem (book : 문제 해결을 위한 알고리즘 with 수학, p.208 문제 4.2.2)

<br>

~~~
어떤 편의점은 0시에 문을 열고,
T시에 문을 닫습니다. 이 편의점에는 N명의
종업원이 일하고 있으며, i번째 종업원은
Li시에 출근해서, Ri시에 퇴근합니다.
이때 Li, Ri는 정수이며,
0 <= Li < Ri <= T를 만족합니다.
t = 0, 1, 2, ..., T-1 각각에 대해서,
t + 0.5시에 편의점에 있는 종업원의
수를 출력하는 프로그램을 작성해주세요.

~~~

<br>

* Input

<br>

~~~
T
N
L1 R1
L2 R2
L3 R3
.
.
.
LN RN
~~~

<br>

~~~
example

10
7
0 3
2 4
1 3
0 3
5 6
5 6
5 6
~~~

<br>

~~~
output

2
3
4
1
0
3
0
0
0
0
~~~

* Solution

<br>

~~~
About input example

1 step : 0 ~ 2 (because the worker will leave at 3) → +1 (increase 1 worker)

Difference = [1, 0, 0, -1, 0, 0, 0, 0, 0, 0]


2 step : 2 ~ 3 → +1 (increase 1 worker)

Difference = [1, 0, 1, -1, -1, 0, 0, 0, 0, 0]

3 step : 1 ~ 2 → +1 (increase 1 worker)

Difference = [1, 1, 1, -2, -1, 0, 0, 0, 0, 0]

4 step : 0 ~ 2 → +1 (increase 1 worker)

Difference = [2, 1, 1, -3, -1, 0, 0, 0, 0, 0]

5 step : 5 ~ 5 → +1 (increase 1 worker)

Difference = [2, 1, 1, -3, -1, 1, -1, 0, 0, 0]

6 step : 5 ~ 5 → +1 (increase 1 worker)

Difference = [2, 1, 1, -3, -1, 2, -2, 0, 0, 0]

7 step : 5 ~ 5 → +1 (increase 1 worker)

Difference = [2, 1, 1, -3, -1, 3, -3, 0, 0, 0]

resDifference = [2, 1, 1, -3, -1, 3, -3, 0, 0, 0]

Consequently, the list for the number of workers at each t is cumulative sum list of resDifference

[2, 3, 4, 1, 0, 3, 0, 0, 0, 0]

~~~
