---
layout: post
title: Mathematical Period
description: >
  Algorithm
tags: [period]
use_math: true
categories:
  - study
  - computer science
---
### Mathematical Period
* Some of problems have specific period mathematically : This is one of strategies for resolving the problems

### Example Problem
* book : 문제 해결을 위한 알고리즘 with 수학, p.310 문제 5.2.3

<br>

~~~
N개의 마을이 있고, 마을에는 1부터 N까지의 번호가 붙어 있습니다. 각각의 마을에는 순간이동 장치가 1대씩 설치되어 있습니다. 마을 i에 있는 순간이동 장치는 마을 Ai로 전송합니다. 마을 1을 시작으로 순간이동 장치를 K번 사용해서, 어떤 마을에 도착하는지 출력하는 프로그램을 작성해 주세요. N <= 200000, K <= 10**18의 범위에서 1초 이내에 답을 구할 수 있게 해 주세요.
~~~

<br>

* Input

<br>


~~~
4 5
3 2 4 1       # meaning : From First village to Third village, From Second village to Second village, From Third village to Fourth village, From Fourth village to First village, we can move
~~~

<br>

* Output

<br>

~~~
4
~~~

<br>

* Sketch

![그림1](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/164.png?raw=true){: width="400" height="800"}<br>

* Code

<br>

~~~python
N, K = map(int, input().split())

graph = input().split()

current = 0
count = 0

First = [-1] * N
Second = [-1] * N

while True:
    if First[current] == -1:
        First[current] = count
    elif Second[current] == -1:
        Second[current] = count

    if count == K:
        print(current + 1)
        flag = False
        break
    elif Second[current] != -1 and (K - First[current]) % (Second[current] - First[current]) == 0:
        print(current + 1)
        break

    current = int(graph[current]) - 1
    count += 1


~~~

<br>
