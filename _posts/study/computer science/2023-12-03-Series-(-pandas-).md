---
layout: post
title: Series (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Series (pandas)
* One data structure of pandas
* index-value (1 : 1 relationship) → similar to **dictionary** (key-value)
* Creation<br>

~~~python
import pandas as pd
s = pd.Series(Dictionary)
~~~

### Index
* There are 2 kinds of index in series
* Integer position index : 0, 1, 2, 3, ...
  * **pandas.Series(list)** → There is no defined index label, so integer index gonna be used automatically
* Index label (user_defined) : key1, key2, key3, ...
  * **pandas.Series(list or tuple, index=[key1, key2, ....])**
* Check index & value<br>

~~~python
import pandas as pd

print(s.index())   # result : RangeIndex(start=n, stop=m, step=a) <m is not included>
print(s.values())  # result : list of values
~~~
<br>

### Access
* Integer Index
  * Series[0] → value0, Series[1] → value1
  * Series[0 : 3] → 0-value0, 1-value1, 2-value2 (series)
  * Series[**[0, 3]**] → 0-value0, 3-value3 (series)
* Index Label
  * Series[key1] → value1
  * Series[key0 : key3] → key0-value0, key1-value1, key2-value2, key3-value3 (series)
  * Series[**[key0, key3]**] → key0-value0, key3-value3 (series)
