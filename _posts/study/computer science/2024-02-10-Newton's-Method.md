---
layout: post
title: Newton's Method
description: >
  Algorithm
tags: [newton's method]
use_math: true
categories:
  - study
  - computer science
---
### Newton's Method
* We can estimate the specific number whose value cannot be determined accurately
* We use tangent line (접선)

### √2 Estimation
* 1<sup>st</sup> : f(x) = x<sup>2</sup> (When x = √2, y = 2) and initial a = 2
* 2<sup>nd</sup> : f<sup> '</sup>(x) = 2x
* 3<sup>rd</sup> : f<sup> '</sup>(a) = 4
* 4<sup>th</sup> : tangent line = 4x - 4
* 5<sup>th</sup> : Intersection point's x coordinate between tangent line and y = 2 → Update a (a = 1.5)
* 6<sup>th</sup> : repeat 2<sup>nd</sup> ~ 5<sup>th</sup>
* That is, intersection point's x coordinate between tangent line and y = 2 gets closer to √2

<br>

~~~python
repeat = 10
a = 2

for i in range(repeat):
    a = (a ** 2 + 2) / (2 * a)             # y = 2ax - a^2 and y = 2

print(a)

~~~

<br>

~~~
1.414213562373095
~~~

### Generalization
* 1<sup>st</sup> : set continuous f(x) (When y = r meets f(x), the intersection x coordinate is target number being going to estimate) and initial a (Initial a should be close to the target number)
* 2<sup>nd</sup> : At point (a, f(a)), tanget line should be calculated
* 3<sup>rd</sup> : The intersection point's x coordinate between the tangent line and y = r should be calculated and it gonna be new a
* 4<sup>th</sup> : repeat
* Example
  * <sup>5</sup>√3 → f(x) = x<sup>5</sup> and y = r = 3
  * e<sup>3</sup> → f(x) = lnx and y = r = 3
  * ln6 → f(x) = e<sup>x</sup> and y = r = 6
  * 5<sup>0.6</sup> → f(x) = x<sup>10</sup> and y = r = 5<sup>6</sup>

### Binary Search
* We can use binary search as well for some estimation
* √2 Estimation
  * 1<sup>st</sup> : Think 1 < √2 < 2
  * 2<sup>nd</sup> : middle = (start + end) / 2 (initial start == 1, initial end == 2)
  * 3<sup>rd</sup> : If m<sup>2</sup> < (√2)<sup>2</sup> = 2, start = m & end = end
  * 4<sup>th</sup> : If m<sup>2</sup> >= (√2)<sup>2</sup> = 2, start = start & end = m
  * 5<sup>th</sup> : Repeat

  <br>

  ~~~python
  def Binary_newton(start, end, count):
    m = (start + end) / 2
    count += 1
    if count == 20:
        return m

    if m ** 2 < 2:
        return Binary_newton(m, end, count)
    else:
        return Binary_newton(start, m, count)

  print(Binary_newton(1, 2, 0))


  ~~~

<br>

* Newton's method vs Binary search
  * Newton's method (repeat = 3) : 5 location correct
  * Binary search (repeat = 20) : 5 location correct
