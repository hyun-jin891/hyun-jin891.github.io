---
layout: post
title: Arithmetic Operation (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Arithmetic Operation (pandas)
* Series with numbers
* Series with series
* Dataframe with numbers
* Dataframe with dataframe

### Series with Numbers
* Series (+, -, *, /) number<br>

~~~python
import pandas as pd

s = pd.Series([200, 150, 80])
s = s / 100

print(s)
~~~
<br>

* The value of elements in s : 2.0, 1.5, 0.8

### Series with series
* Series (+, -, *, /) Series
* Operation happens between **same index** (not order) of each series
* If some indexes are not involved in either one series or the other series, its value will be NaN
  * For filling the value instead of NaN, we can use operation method (add, sub, mul, div) with a parameter fill_value
  * **s1.add(s2, fill_value = 0)**
  * **s1.sub(s2, fill_value = 0)**
  * **s1.mul(s2, fill_value = 0)**
  * **s1.div(s2, fill_value = 0)**

### Dataframe with Number
* Dataframe (+, -, *, /) number

### Dataframe with Dataframe
* Dataframe (+, -, *, /) Dataframe
* Operation happens between **same index** (not order) of each dataframe
* If some indexes are not involved in either one series or the other series, its value will be NaN
  * For filling the value instead of NaN, we can use operation method (add, sub, mul, div) with a parameter fill_value
