---
layout: post
title: Filtering (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Filtering (pandas)
* Extract the records which satisfy the condition from dataframe
* Method
  * Boolean Indexing
  * isin

### Boolean Indexing
* Condition should be expressed by boolean data
  * ex1.
  ~~~
  condition = df['column name'] > 20 : I wanna extract the records whose 'column name' data is bigger than 20
  ~~~
  * ex2.
  ~~~
  condition = df['column name'] == 10 : I wanna extract the records whose 'column name' data is 10
  ~~~
  * ex3.
  ~~~
  condition = (df['column name'] > 10) & (df['column name'] < 20) : I wanna extract the records whose 'column name' is between 10 and 20
  ~~~
  * ex4.
  ~~~
  condition = (df['column name'] < 10) | (df['column name'] > 20) : I wanna extract the records whose 'column name' is smaller than 10 or bigger than 20
  ~~~
* Boolean Indexing
  * **df = df[condition]** or **df = df.loc[condition, :]**
    * extract the records which satisfy the condition and make dataframe
  * **df = df.loc[condition, ['c1', 'c2', ...]]**
    * extract the records which satisfy the condition and make dataframe whose column is only ['c1', 'c2', ...] (not all columns)

~~~python
import pandas as pd

df = pd.DataFrame({'c1' : [30, 21, 30], 'c2' : [40, 50, 50], 'c3' : [70, 50, 60]})

condition = df['c1'] > 25

df1 = df[condition]
df2 = df.loc[condition, :]
df3 = df.loc[condition, ['c1', 'c2']]

print(df1)
print()
print(df2)
print()
print(df3)
~~~

<br>

~~~
c1  c2  c3
0  30  40  70
2  30  50  60

c1  c2  c3
0  30  40  70
2  30  50  60

c1  c2
0  30  40
2  30  50
~~~
<br>

### isin
* This is used when we wanna extract the records whose the specific column contains the specific values
  * If we use boolean indexing to this situation, condition = df['column name'] == n
* **condition = df['column name'].isin([v1, v2, v3])**
  * We wanna extract the records whose 'column name' data is v1 or v2 or v3
* **df = df[condition]** or **df = df.loc[condition, :]**
  * extract the records which satisfy the condition and make dataframe
* **df = df.loc[condition, ['c1', 'c2', ...]]**
  * extract the records which satisfy the condition and make dataframe whose column is only ['c1', 'c2', ...] (not all columns)

<br>

~~~python
import pandas as pd

df = pd.DataFrame({'c1' : [30, 21, 30], 'c2' : [40, 50, 50], 'c3' : [70, 50, 60]})

condition = df['c3'].isin([70, 60])

df1 = df[condition]
df2 = df.loc[condition, :]
df3 = df.loc[condition, ['c1', 'c2']]

print(df1)
print()
print(df2)
print()
print(df3)
~~~


~~~
c1  c2  c3
0  30  40  70
2  30  50  60

c1  c2  c3
0  30  40  70
2  30  50  60

c1  c2
0  30  40
2  30  50
~~~
