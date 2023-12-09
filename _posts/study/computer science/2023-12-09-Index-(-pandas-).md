---
layout: post
title: Index (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Index (pandas)
* Choose the columns as the index of dataframe (set index)
* Initialization of index
* Rearrange index
* Sorting

### Set Index
* Specific columns can be set as the index of dataframe
* Single index
  * **df.set_index("column2")** (column2 gonna be the index of this dataframe)
* Multiple index
  * **df.set_index(["column1", "column3"])** (Both column1 and column3 gonna be the index of this dataframe)
* If set_index is used, original index gonna be deleted from its dataframe

### Initialization of Index
* Use "reset_index"
  * origin source changes (X) → just return
  * If we wanna change the origin dataframe, we insert "inplace=True" as a parameter
* **df.reset_index()**
  * Integer position index gonna be the index of dataframe
  * Original index gonna be the first column (The column's name is "index")

### Rearrange Index
* If we wanna change the order of index, use "reindex"
  * origin source changes (X) → just return
  * "inplace = True" cannat be used
<br>

~~~python
import pandas as pd

df = pd.DataFrame({'math' : [100, 20, 98], 'english' : [15, 75, 68], 'biology' : [98, 100, 95]}, index = ['Samoh', 'Rivera', 'Cho'])
df = df.reindex(['Cho', 'Samoh', 'Rivera'])

print(df)
~~~
<br>

* The order of index gonna be changed to 'Cho', 'Samoh', 'Rivera'<br>

~~~python
import pandas as pd

df = pd.DataFrame({'math' : [100, 20, 98], 'english' : [15, 75, 68], 'biology' : [98, 100, 95]}, index = ['Samoh', 'Rivera', 'Cho'])
df = df.reindex(['Cho', 'Samoh', 'Rivera', 'Sanford', 'Westberry'])

print(df)
~~~
<br>

* 'Sanford' and 'Westberry' are not involved in the original dataframe : They gonna be added as new index (all of elements are NaN)
* NaN can be replaced if we insert fill_value parameter to reindex method
  * **df.reindex(index_list, fill_value = 3)** (If some elements in index_list are not involved in the original dataframe, they gonna be added as new index and their elements' value is 3)

### Sorting Based on Index
* Based on index, rows of dataframe can be sorted
* Use "sort_index"
  * origin source changes (X) → just return
  * If we wanna change the origin dataframe, we insert "inplace=True" as a parameter
* **df.sort_index(ascending=True)**
* **df.sort_index(ascending=False)**

~~~python
import pandas as pd

df = pd.DataFrame({'math' : [100, 20, 98], 'english' : [15, 75, 68], 'biology' : [98, 100, 95]}, index = ['Samoh', 'Rivera', 'Cho'])

df = df.sort_index(ascending=True)

print(df)
~~~
<br>

* The order of index : 'Cho', 'Rivera', 'Samoh'

### Sorting Based on Value
* We can sort the dataframe by choosing a single column → It is sorted using the value of elements in this column
* Use "sort_values"
  * origin source changes (X) → just return
  * If we wanna change the origin dataframe, we insert "inplace=True" as a parameter
<br>

~~~python
import pandas as pd

df = pd.DataFrame({'math' : [100, 20, 98], 'english' : [15, 75, 68], 'biology' : [98, 100, 95]}, index = ['Samoh', 'Rivera', 'Cho'])

df = df.sort_values(by='math', ascending=False)

print(df)
~~~
<br>

* By the value of elements in math, the order of index is 'Samoh', 'Cho', 'Rivera'
