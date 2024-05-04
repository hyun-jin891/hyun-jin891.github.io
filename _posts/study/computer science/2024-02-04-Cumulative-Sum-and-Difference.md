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
This is the private repository where I can only review my notes<br>
[Repository](https://github.com/hyun-jin891/hidden-post-hyunjin891-github-blog/blob/master/_posts/study/computer%20science/2024-02-04-Cumulative-Sum-and-Difference.md)

### Example of Difference
This is the private repository where I can only review my notes<br>
[Repository](https://github.com/hyun-jin891/hidden-post-hyunjin891-github-blog/blob/master/_posts/study/computer%20science/2024-02-04-Cumulative-Sum-and-Difference.md)
