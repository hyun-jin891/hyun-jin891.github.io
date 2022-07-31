---
layout: post
title: Measure for center
description: >
  mean, median, mode
tags: [measure for center]
use_math: true
categories:
  - study
  - statistics
---
## Measure For Center
* Mean
* Median
* Mode

## Mean
* Population mean : calculation of mean in population<br>
![그림1](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/73.PNG?raw=true){: width="600" height="600"}<br>

~~~
Example
  Population A
  10, 15, 28, 58, 59, 67, 89, 104, 14, 57, 65, 105

  μ = (10 + 15 + 28 + 58 + 59 + 67 + 89 + 104 + 14 + 57 + 65 + 105) / 12 = 55.91667
~~~

* Sample mean : calculation of mean in a specific sample<br>
![그림2](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/74.PNG?raw=true){: width="600" height="600"}<br>

~~~
Example
  Sample (from population A) : n = 5
  58, 67, 89, 14, 57

  Sample mean (X-bar) = (58 + 67 + 89 + 14 + 57) / 5 = 285
~~~

* Trimmed mean (절사 평균)
  * Data should be sorted
  * α% trimmed mean : After removing (n x 0.0α) data from each side, calculate the mean of data
  * If (n x 0.0α) is not integer (m.xxx) : remove m data from each side → calculate the mean, but each end data should be multiplied by 0.xxx<br>

~~~
Example
  Population A
  10, 15, 28, 58, 59, 67, 89, 104, 14, 57, 65, 105

  10% trimmed mean
  → sorting : 10 14 15 28 57 58 59 65 67 89 104 105
  → (14 x 0.2) + 15 + 28 + 57 + 58 + 59 + 65 + 67 + 89 + (104 x 0.2) / 10 = 461.6
~~~

## Properties Of Mean
* **Sensitive to outlier**<br>
→ can use trimmed mean instead of general mean

## Median
* Data should be sorted
* Middle position of data
* if n is odd : (n + 1)/2
* if n is even : average for n/2 and n/2 + 1
* not sensitive to outlier <br>

~~~
Example 1
  Population A
  10, 15, 28, 58, 59, 67, 89, 104, 14, 57, 65, 105

  → sorting : 10 14 15 28 57 58 59 65 67 89 104 105
  Median = (58 + 59)/2 = 58.5
~~~

~~~
Example 2
  Sample (from population A) : n = 5
  58, 67, 89, 14, 57

  → sorting : 14 57 58 67 89
  Median = 58/2 = 29
~~~

## Mode
* Data that have the highest frequency<br>

~~~
Example
  Population B
  8, 9, 20, 9, 8, 2, 35, 35, 9, 50, 9, 11, 9

  Mode = 9
~~~

## Estimation For Distribution
* **Mean is sensitive to outlier**
* Symmetric<br>
![그림3](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/75.PNG?raw=true){: width="600" height="600"}<br>

* Skewed to the right<br>
![그림4](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/76.PNG?raw=true){: width="600" height="600"}<br>

* Skewed to the left<br>
![그림5](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/77.PNG?raw=true){: width="600" height="600"}<br>
