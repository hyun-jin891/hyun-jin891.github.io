---
layout: post
title: Dataframe (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Dataframe (pandas)
* One data structure of pandas
* Like matrix, there are rows (each row (= a **record**) name : **index**) and columns
* Creation<br>

~~~python
import pandas as pd

df1 = pd.DataFrame({"math" : [90, 74, 65], "biology" : [100, 60, 97], "chemistry" : [85, 100, 78]}, index = ["Samoh", "Rivera", "Cho"]) # if you don't insert index content, the default index is integer index

df2 = pd.DataFrame([[90, 100, 85], [74, 60, 100], [65, 97, 78]], index = ["Samoh", "Rivera", "Cho"], columns = ["math", "biology", "chemistry"])
~~~
<br>
* In df2, [90, 100, 85] is a record<br>
* A person name : a single index<br>
* A subject name : a single columns<br>
* We can make dataframe like this way
  * df = pd.DataFrame([Series1, Series2, ...], index = ["index0", "index1", ...])

### Change Index & Columns
* print(df.index) : check the index of df
* print(df.columns) : check the columns' names
* change the names of all of things in index & columns
  * **df.index = [i0, i1, i2, ....]**
  * **df.columns = [c0, c1, c2, ....]**
* only change the parts that we wanna change
  * Use "rename"
    * origin source changes (X) → just return
    * If we wanna change the origin dataframe, we insert "inplace=True" as a parameter
  * **df.rename(index={originI0 : newI0, originI1 : newI1, originI2 : newI2, ...})**
  * **df.rename(columns={originC0 : newC0, originC1 : newC1, ...})**

### Deletion of Rows & Columns
* Use "drop"
  * origin source changes (X) → just return
  * If we wanna change the origin dataframe, we insert "inplace=True" as a parameter
* **df.drop('index0', axis=0)** (remove a row whose name is index0, default : axis=0)
* **df.drop(['index0', 'index3'], axis=0)** (remove 2 rows)
* **df.drop('column0', axis=1)** (remove a column whose name is column0)
* **df.drop(['column0', 'column3'], axis=1)** (remove 2 columns)

### Choose Rows
* Use "loc" (index label) & "iloc" (integer index)
* Choose a single row
  * **df.loc['index0']** (return Series corresponding to index0)
  * **df.loc[0]** (return Series corresponding to first index)
* Choose multiple rows
  * Double times [] & Slicing returns dataframe
    * df.loc['index0'] != df.loc[**['index0']**]
  * **df.loc[**['index0', 'index3']**]** (return dataframe corresponding to index0 & index3)
  * **df.loc['index0' : 'index3']** (return dataframe corresponding to index0 & index1 & index2 & index3)
  * **df.iloc[**[0, 3]**]** (return dataframe corresponding to first and fourth index)
  * **df.iloc[0 : 4]** (return dataframe corresponding to first, second, third, and fourth index)
    * **df.iloc[0 : 4 : 2]** (return dataframe corresponding to first, third index)
    * **df.iloc[ : : -1]** (return dataframe corresponding to last, last - 1, last - 2, ..., first index)

### Choose Columns
* Choose a single column
  * **df['column1']** (return Series corresponding to column1)
  * **df.column2** (return Series corresponding to column2)
* Choose multiple columns
  * **df[**['column1', 'column2', 'column5']**]** (return dataframe corresponding to column1, column2, and column5)

### Choose Element
* **df.(i)loc[row information, column information]**
  * Example
    * **df.iloc[1, 2]** (return a single element corresponding to second index and third column)
    * **df.loc['index2', 'column2']** (return a single element corresponding to index2 and column2)
    * **df.iloc[1, 2 : 5]** (return Series corresponding to second index and third, fourth, fifth column)
    * **df.loc['index2', **['column1', 'column3']**]** (return Series corresponding to index2 and column1, column3)
    * **df.iloc[1 : 3, 2 : 5]** (return dataframe corresponding to second, third index and third, fourth, fifth column)
    * **df.loc['index2' : 'index4', **['column1', 'column3']**]** (return dataframe corresponding to index2, index3, index4 and column1, column3)
    * **df.iloc[:, 2 : 5]** (return dataframe corresponding to second index and third, fourth, fifth column)
    * **df.loc[:, **['column1', 'column3']**]** (return dataframe corresponding to column1, column3)
* **df.(i)loc[**row information**][**column information**]**

### Add Row
* New row has only one value as element
  * **df.loc["index5"] = 0** (add a row named as index5 and this row has only 0 as element)
* New row has various values as element
  * **df.loc["index5"] = [2, 4, 5, 7]** (Series is also possible) (add a row named as index5 and [2, 4, 5, 7] is a record)

### Add Column
* New column has only one value as element
  * **df["column5"] = 0** (add a column named as column5 and this column has only 0 as element)
* New row has various values as element
  * **df["column5"] = [2, 4, 5, 7]** (Series is also possible) (add a column named as column5)

### Change Element
* **(Choose parts that we wanna change) = newValue**
  * Example
    * **df.iloc[1, 2] = 50** (Change the original value located on second index, thrid column to 50)
    * **df.loc['index1', **['column2', 'column4']**] = [40, 60]** (Change the original values located on (second index, column2), (second index, column4) to 40 and 60)

### Transpose
* row → column, column → row
* **df.transpose()**
* **df.T**

### Head & Tail
* **df.head(number)** (return dataframe with n rows starting from the first row, default : number == 5)
* **df.tail(number)** (return dataframe with n rows starting from the last row, default : number == 5)
