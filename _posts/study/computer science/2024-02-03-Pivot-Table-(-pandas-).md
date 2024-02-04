---
layout: post
title: Pivot Table (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Pivot Table (pandas)
* choose the row index, column index, value that are used for new DataFrame from original DataFrame's column index + Use aggregation method → Make new DataFrame (pivot table)

### Making Pivot Table
* **pd.pivot_table(df, index='specific column1', columns='specific column2', values='specific column3', aggfunc='method')** : For each option (except df), multiple elements can be allowed

<br>

~~~python
import pandas as pd
import seaborn as sns


titanic = sns.load_dataset('titanic')

df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

newDF1 = pd.pivot_table(df, index='pclass', columns='survived', values='fare', aggfunc='mean')
newDF2 = pd.pivot_table(df, index=['pclass', 'sex'], columns='survived', values='fare', aggfunc=['mean', 'sum'])
newDF3 = pd.pivot_table(df, index=['pclass', 'sex'], columns='survived', values=['fare', 'age'], aggfunc=['mean', 'sum'])

print(newDF1)
print()
print(newDF2)
print()
print(newDF3)

~~~

<br>

~~~
survived          0          1
pclass
1         64.684008  95.608029
2         19.412328  22.055700
3         13.669364  13.694887

                     mean                    sum
survived                0           1          0          1
pclass sex
1      female  110.604167  105.978159   331.8125  9644.0125
       male     62.894910   74.637320  4842.9081  3358.6794
2      female   18.250000   22.288989   109.5000  1560.2292
       male     19.488965   21.095100  1773.4958   358.6167
3      female   19.773093   12.464526  1423.6627   897.4459
       male     12.204469   15.579696  3661.3408   732.2457

                    mean                                        sum
                     age                   fare                 age                fare
survived               0          1           0           1       0        1          0          1
pclass sex
1      female  25.666667  34.939024  110.604167  105.978159    77.0  2865.00   331.8125  9644.0125
       male    44.581967  36.248000   62.894910   74.637320  2719.5  1449.92  4842.9081  3358.6794
2      female  36.000000  28.080882   18.250000   22.288989   216.0  1909.50   109.5000  1560.2292
       male    33.369048  16.022000   19.488965   21.095100  2803.0   240.33  1773.4958   358.6167
3      female  23.818182  19.329787   19.773093   12.464526  1310.0   908.50  1423.6627   897.4459
       male    27.255814  22.274211   12.204469   15.579696  5860.0   846.42  3661.3408   732.2457

~~~

<br>

### Multiindex
* In grouping or pivot table and so on, there can be multiindex dataframe
* Accessing method
  * loc
    * **df.loc['specific index from first index']**
      * extract the dataframe corresponding to the specific index (First index of multiindex)
      * If we directly put the index not from first index, error happens
      * Utilize **df.loc[('specific index from first index', 'specific index from second index', 'specific index from third index', ...)]** or **df.loc[[(index1 from first index, index1 from second index, ...), (index2 from first index, index2 from second index, ...), ...]]**

  <br>

  ~~~python
  import pandas as pd
  import seaborn as sns


  titanic = sns.load_dataset('titanic')

  df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

  newDF3 = pd.pivot_table(df, index=['pclass', 'sex'], columns='survived', values=['fare', 'age'], aggfunc=['mean', 'sum'])

  print(newDF3.loc[1])
  print()
  print(newDF3.loc[(1, 'male')])

  ~~~

  <br>

  ~~~
                  mean                                        sum
                   age                   fare                 age                fare
   survived          0          1           0           1       0        1          0          1
   sex
   female    25.666667  34.939024  110.604167  105.978159    77.0  2865.00   331.8125  9644.0125
   male      44.581967  36.248000   62.894910   74.637320  2719.5  1449.92  4842.9081  3358.6794

   survived
   mean  age   0             44.581967
               1             36.248000
         fare  0             62.894910
               1             74.637320
   sum   age   0           2719.500000
               1           1449.920000
         fare  0           4842.908100
               1           3358.679400
   Name: (1, male), dtype: float64
  ~~~

  <br>

  * xs
    * **df.xs('specific index', level='First index or Second index or Third index or ...')**
    * Unlike loc, xs method can directly access to second, third, ... index

  <br>

  ~~~python
  import pandas as pd
  import seaborn as sns


  titanic = sns.load_dataset('titanic')

  df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

  newDF = pd.pivot_table(df, index=['pclass', 'sex', 'embark_town'], columns='survived', values=['fare', 'age'], aggfunc=['mean', 'sum'])

  print(newDF.xs('Queenstown', level='embark_town'))

  ~~~

  <br>

  ~~~
                       mean                                sum
                        age             fare               age            fare
   survived               0     1          0          1      0     1         0         1
   pclass sex
   1      female        NaN  33.0        NaN  90.000000    NaN  33.0       NaN   90.0000
          male    44.000000   NaN  90.000000        NaN   44.0   NaN   90.0000       NaN
   2      female        NaN  30.0        NaN  12.350000    NaN  30.0       NaN   24.7000
          male    57.000000   NaN  12.350000        NaN   57.0   NaN   12.3500       NaN
   3      female  28.100000  17.6  10.904633  10.084033  140.5  88.0   98.1417  242.0168
          male    28.076923  29.0  11.841550  12.916667  365.0  29.0  426.2958   38.7500
  ~~~

  <br>

  ~~~python
  import pandas as pd
  import seaborn as sns


  titanic = sns.load_dataset('titanic')

  df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

  newDF = pd.pivot_table(df, index=['pclass', 'sex', 'embark_town'], columns='survived', values=['fare', 'age'], aggfunc=['mean', 'sum'])

  print(newDF.xs(('male', 'Queenstown'), level=['sex','embark_town']))

  ~~~

  <br>

  ~~~
                  mean                               sum
                   age            fare               age            fare
   survived          0     1         0          1      0     1         0      1
   pclass
   1         44.000000   NaN  90.00000        NaN   44.0   NaN   90.0000    NaN
   2         57.000000   NaN  12.35000        NaN   57.0   NaN   12.3500    NaN
   3         28.076923  29.0  11.84155  12.916667  365.0  29.0  426.2958  38.75
  ~~~
