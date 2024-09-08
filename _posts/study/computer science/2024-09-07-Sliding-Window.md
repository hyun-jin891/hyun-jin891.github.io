---
layout: post
title: Sliding Window
description: >
  Algorithm
tags: [sliding window]
use_math: true
categories:
  - study
  - computer science
---
### Sliding Window
* Given the array, it is used when we need to deal with something within the specific sections
* For sliding window, we let the sections with the constant size move one by one
  * **remove the first element of the section + add the new element**
  * It's like **"Queue"**

### Sliding Window Example
* Sum

~~~
[1, 3, 2, 7, 8, 9, 10]
 -------

 sum = 1 + 3 + 2 = 6
~~~

~~~
[1, 3, 2, 7, 8, 9, 10]
    -------

 sum = 6 - 1 + 7 = 12
~~~

~~~
[1, 3, 2, 7, 8, 9, 10]
       -------

 sum = 12 - 3 + 8 = 17
~~~

~~~
[1, 3, 2, 7, 8, 9, 10]
          -------

 sum = 17 - 2 + 9 = 24
~~~

~~~
[1, 3, 2, 7, 8, 9, 10]
             -------

 sum = 24 - 7 + 10 = 27
~~~
