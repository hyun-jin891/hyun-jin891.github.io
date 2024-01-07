---
layout: post
title: Function Mapping (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Function Mapping (pandas)
* Series.apply(function)
  * function's parameter : multiple individual values (first parameter should be Series' element itself)
  * Elements from Series are calculated by the function
* DataFrame.applymap(function)
  * function's parameter : a single individual value
  * Elements from DataFrame are calculated by the function
* DataFrame.apply(function)
  * function's parameter : a Series
  * Each column (axis=0) or row (axis=1) is calculated by the function
* DataFrame.pipe(function)
  * function's parameter : a DataFrame
  * Dataframe itself is calculated by the function

### Mapping Result
* It gonna be different as both the type of result from mapped function and that of object which wanna use mapping method
* Example
  * If the result from mapped function is an individual value and we use Series.apply → return Series
  * If the result from mapped function is a Series and we use Series.apply → return DataFrame (Each result from the function gonna be record of the DataFrame)
  * If the result from mapped function is a DataFrame and we use DataFrame.pipe → return DataFrame

### Series.apply
* The parameter of function used to do mapping should be a single (or multiple) individual value
* **Series' name.apply(function's name, second parameter=value2, Third parameter=value3, ...)**
  * Same result : **Series' name.apply(lambda x : function's name(x, value2, value3, ...))**
* First parameter of function is for elements from Series

* Example Code
<br>

~~~python
import pandas as pd

def func(element, b, c):
  return element + b + c

df = pd.DataFrame({'c1' : [10, 20, 30], 'c2' : [40, 50, 50], 'c3' : [70, 50, 60]})

newSeries = df['c1'].apply(func, b=20, c=30) # Same result : newSeries = df['c1'].apply(lambda x : func(x, 20, 30))

print(newSeries)
~~~

<br>

~~~
0    60
1    70
2    80
Name: c1, dtype: int64
~~~

### DataFrame.applymap
* The parameter of function used to do mapping should be a single individual value (multiple X)
* **DataFrame's name.applymap(function's name)**
  * **DataFrame's name.applymap(lambda x : function's name(x))**
* Example Code
<br>

~~~python
import pandas as pd

def func(element):
  return element - 15

df = pd.DataFrame({'c1' : [10, 20, 30], 'c2' : [40, 50, 50], 'c3' : [70, 50, 60]})

newDF = df.applymap(func) # Same result : newDF = df.applymap(lambda x : func(x))

print(newDF)

~~~

<br>

~~~
   c1  c2  c3
0  -5  25  55
1   5  35  35
2  15  35  45
~~~

### DataFrame.apply
* The parameter of function used to do mapping should be a Series
* **DataFrame's name.apply(function's name, axis=0)**
  * **DataFrame's name.apply(lambda x : function's name(x), axis=0)**
  * Each column (axis=0) is calculated by the function
* **DataFrame's name.apply(function's name, axis=1)**
  * **DataFrame's anme.apply(lambda x : function's name(x), axis=1)**
  * Each row (axis=1) is calculated by the function
* Example Code
<br>

~~~python
import pandas as pd

def func1(Series):
  return Series + 15

def func2(Series):
  return Series.max()

df = pd.DataFrame({'c1' : [10, 20, 30], 'c2' : [40, 50, 50], 'c3' : [70, 50, 60]})

newDF1 = df.apply(func1, axis=0)
newDF2 = df.apply(lambda x : func1(x[['c2', 'c3']]), axis=1)  # We can limit the column range of each row which gonna be calculated by the function using lambda when axis is 1

newSeries = df.apply(func2, axis=0)


print(newDF1)
print()
print(newDF2)
print()
print(newSeries)

~~~

<br>

~~~
   c1  c2  c3
0  25  55  85
1  35  65  65
2  45  65  75

   c2  c3
0  55  85
1  65  65
2  65  75

c1    30
c2    50
c3    70
dtype: int64
~~~

### DataFrame.pipe
* The parameter of function used to do mapping should be a DataFrame
* **DataFrame's name.pipe(function's name)**
  * **DataFrame's name.pipe(lambda x : function's name(x))**
* Example Code
<br>

~~~python
import pandas as pd

def func1(DF):
  return DF + 20

def func2(DF):
  return DF.sum(axis=0)

def func3(DF):
  return DF.max(axis=0).max()

df = pd.DataFrame({'c1' : [10, 20, 30], 'c2' : [40, 50, 50], 'c3' : [70, 50, 60]})

newDF = df.pipe(func1)
newSeries = df.pipe(func2)
resValue = df.pipe(func3)

print(newDF)
print()
print(newSeries)
print()
print(resValue)

~~~

<br>

~~~
   c1  c2  c3
0  30  60  90
1  40  70  70
2  50  70  80

c1     60
c2    140
c3    180
dtype: int64

70
~~~
