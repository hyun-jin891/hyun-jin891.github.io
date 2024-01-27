---
layout: post
title: Changing Column (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Changing Column (pandas)
* Changing order
* Split

### Changing Order
* Extract current column list
  * **columns = list(df.columns.values)**
* Sorting the column list
  * **columns = sorted(list(df.columns.values))**
    * Dictionary order
  * **columns = list(reversed(list(df.columns.values)))**
    * reverse order of orginal column list (not reverse order of dictionary)
* Create column list directly
  * **columns = ['c1', 'c2', ...]**
* **Apply** the change of column list to dataframe
  * **df = df[columns]**

### Split
* We wanna split one column into several columns (One datum of the one column should be separated, ex. 2001-05-20 → [2001, 05, 20])
* First of all, the data type should be 'str'
  * **df['c1'] = df['c1'].astype('str')**
* Second, we make Series representing the result list like [2001, 05, 20]
  * **s = df['c1'].str.split("special character")**
  * Based on special character, we split the data
* Third, we make Series which gonna be new column
  * **df['new column'] = s.str.get(index)**
  * ex. [year, month, days] → get(0) : year, get(1) : month, get(2) : days

<br>

~~~python
import pandas as pd

df = pd.DataFrame({'c1' : ["1/28/30", "5/14/21", "10/20/30"], 'c2' : [40, 50, 50], 'c3' : [70, 50, 60]})

df['c1'] = df['c1'].astype('str')

s = df['c1'].str.split("/")

df['first'] = s.str.get(0)
df['second'] = s.str.get(1)
df['third'] = s.str.get(2)

print(s)
print()
print(df)
~~~
<br>

~~~
0     [1, 28, 30]
1     [5, 14, 21]
2    [10, 20, 30]
Name: c1, dtype: object

         c1  c2  c3 first second third
0   1/28/30  40  70     1     28    30
1   5/14/21  50  50     5     14    21
2  10/20/30  50  60    10     20    30
~~~
