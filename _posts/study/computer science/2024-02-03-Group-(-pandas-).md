---
layout: post
title: Group (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Group (pandas)
* 1<sup>st</sup> : split data by the specific standard (grouping)
* 2<sup>nd</sup> : The methods for data aggregation or data change are applied to each group (Mapping applied to group : agg, transform, apply, filter)
* 3<sup>rd</sup> : combine (Two Dataframe Connection posting)

### Grouping
* split data by the specific standard
* **df.groupby(["standard column1", "sc2", "sc3", ...])**
  * It returns a object that has key and each group dataframe
  * key : (A value from standard column1, A value from sc2, ....)
  * group dataframe : Dataframe corresponding to a specific key
  * The index on each record is not changed

<br>

~~~python
import pandas as pd
import seaborn as sns

titanic = sns.load_dataset('titanic')

df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

groups = df.groupby(['pclass'])

for key, group in groups:
    print("pclass :", key)
    print('\n')
    print(group.head())
    print('\n')

~~~

<br>

~~~
pclass : 1


    pclass   age     sex     fare  survived  embark_town
1        1  38.0  female  71.2833         1    Cherbourg
3        1  35.0  female  53.1000         1  Southampton
6        1  54.0    male  51.8625         0  Southampton
11       1  58.0  female  26.5500         1  Southampton
23       1  28.0    male  35.5000         1  Southampton


pclass : 2


    pclass   age     sex     fare  survived  embark_town
9        2  14.0  female  30.0708         1    Cherbourg
15       2  55.0  female  16.0000         1  Southampton
17       2   NaN    male  13.0000         1  Southampton
20       2  35.0    male  26.0000         0  Southampton
21       2  34.0    male  13.0000         1  Southampton


pclass : 3


   pclass   age     sex     fare  survived  embark_town
0       3  22.0    male   7.2500         0  Southampton
2       3  26.0  female   7.9250         1  Southampton
4       3  35.0    male   8.0500         0  Southampton
5       3   NaN    male   8.4583         0   Queenstown
7       3   2.0    male  21.0750         0  Southampton

~~~

<br>

~~~python
import pandas as pd
import seaborn as sns

titanic = sns.load_dataset('titanic')

df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

groups = df.groupby(['pclass', 'sex'])

for key, group in groups:
    print("(pclass, sex) :", key)
    print('\n')
    print(group.head())
    print('\n')
~~~

<br>

~~~
(pclass, sex) : (1, 'female')


    pclass   age     sex      fare  survived  embark_town
1        1  38.0  female   71.2833         1    Cherbourg
3        1  35.0  female   53.1000         1  Southampton
11       1  58.0  female   26.5500         1  Southampton
31       1   NaN  female  146.5208         1    Cherbourg
52       1  49.0  female   76.7292         1    Cherbourg


(pclass, sex) : (1, 'male')


    pclass   age   sex      fare  survived  embark_town
6        1  54.0  male   51.8625         0  Southampton
23       1  28.0  male   35.5000         1  Southampton
27       1  19.0  male  263.0000         0  Southampton
30       1  40.0  male   27.7208         0    Cherbourg
34       1  28.0  male   82.1708         0    Cherbourg


(pclass, sex) : (2, 'female')


    pclass   age     sex     fare  survived  embark_town
9        2  14.0  female  30.0708         1    Cherbourg
15       2  55.0  female  16.0000         1  Southampton
41       2  27.0  female  21.0000         0  Southampton
43       2   3.0  female  41.5792         1    Cherbourg
53       2  29.0  female  26.0000         1  Southampton


(pclass, sex) : (2, 'male')


    pclass   age   sex  fare  survived  embark_town
17       2   NaN  male  13.0         1  Southampton
20       2  35.0  male  26.0         0  Southampton
21       2  34.0  male  13.0         1  Southampton
33       2  66.0  male  10.5         0  Southampton
70       2  32.0  male  10.5         0  Southampton


(pclass, sex) : (3, 'female')


    pclass   age     sex     fare  survived  embark_town
2        3  26.0  female   7.9250         1  Southampton
8        3  27.0  female  11.1333         1  Southampton
10       3   4.0  female  16.7000         1  Southampton
14       3  14.0  female   7.8542         0  Southampton
18       3  31.0  female  18.0000         0  Southampton


(pclass, sex) : (3, 'male')


    pclass   age   sex     fare  survived  embark_town
0        3  22.0  male   7.2500         0  Southampton
4        3  35.0  male   8.0500         0  Southampton
5        3   NaN  male   8.4583         0   Queenstown
7        3   2.0  male  21.0750         0  Southampton
12       3  20.0  male   8.0500         0  Southampton
~~~

<br>

* Accessing each group dataframe
  * **groups.get_group("key")** (if key is numerical type, remove "")

<br>

~~~python
import pandas as pd
import seaborn as sns

titanic = sns.load_dataset('titanic')

df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

groups = df.groupby(['pclass'])

group = groups.get_group(2)

print(group.head())

~~~

<br>

~~~
    pclass   age     sex     fare  survived  embark_town
9        2  14.0  female  30.0708         1    Cherbourg
15       2  55.0  female  16.0000         1  Southampton
17       2   NaN    male  13.0000         1  Southampton
20       2  35.0    male  26.0000         0  Southampton
21       2  34.0    male  13.0000         1  Southampton
~~~

<br>

~~~python
import pandas as pd
import seaborn as sns

titanic = sns.load_dataset('titanic')

df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

groups = df.groupby(['pclass', 'sex'])
group = groups.get_group((2, 'male'))

print(group.head())

~~~

<br>

~~~
    pclass   age   sex  fare  survived  embark_town
17       2   NaN  male  13.0         1  Southampton
20       2  35.0  male  26.0         0  Southampton
21       2  34.0  male  13.0         1  Southampton
33       2  66.0  male  10.5         0  Southampton
70       2  32.0  male  10.5         0  Southampton
~~~

<br>

### agg
* The methods for data aggregation or data change are applied to each group (**agg**, transform, apply, filter)
* **agg** method can be used only for **aggregation method** (집계 메소드, ex. mean(), max(), sum(), count(), size(), var(), std(), describe(), info(), first() (extract the first record from each group and connect them), last() (extract the last record from each group and connect them))
  * **groups.agg(mainly user-defined aggregation method)**
  * The result dataframe's indexes are keys
  * The parameter of a function used for mapping is a group dataframe
  * If there are some column series that cannot be applied by aggregation method (ex. type is different), it ignores them and automatically calculate only desired columns
  * **groups.specific column.agg(aggregation method)**
    * The result is Series representing the result from mapping applied to only specific column

  <br>

  ~~~python
  import pandas as pd
  import seaborn as sns

  def func(x):                # x → each group's dataframe
      return x.mean() + x.min()

  titanic = sns.load_dataset('titanic')

  df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

  groups = df.groupby(['pclass'])

  resDF1 = groups.agg(func)
  resDF2 = groups.age.agg(func)

  print(resDF1)
  print()
  print(resDF2)

  ~~~

  <br>

  ~~~
  age       fare  survived
  pclass
  1       39.153441  84.154687  0.629630
  2       30.547630  20.662183  0.472826
  3       25.560620  13.675550  0.242363

  pclass
  1    39.153441
  2    30.547630
  3    25.560620
  Name: age, dtype: float64
  ~~~

  <br>

  * We can use multiple aggregation methods with agg method
    * **groups.agg([method1, method2, method3, ....])** : Multiple methods are applied to all columns
    * **groups.agg({'column1' : method1, 'c2' : [method1, method2], ....})** : Method1 is applied to column1 and Both method1 and method2 are applied to column2, ....

    <br>

    ~~~python
    import pandas as pd
    import seaborn as sns

    def func(x):                
        return x.mean() + x.min()

    titanic = sns.load_dataset('titanic')

    df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

    groups = df.groupby(['pclass'])


    resDF1 = groups.agg([func, max])
    resDF2 = groups.agg({'age' : [func, max], 'fare' : min, 'survived' : max})


    print(resDF1)
    print()
    print(resDF2)

    ~~~

    <br>

    ~~~
                  age             fare            survived    
                 func   max       func       max      func max
    pclass
    1       39.153441  80.0  84.154687  512.3292  0.629630   1
    2       30.547630  70.0  20.662183   73.5000  0.472826   1
    3       25.560620  74.0  13.675550   69.5500  0.242363   1

                    age       fare survived
                   func   max  min      max
      pclass
      1       39.153441  80.0  0.0        1
      2       30.547630  70.0  0.0        1
      3       25.560620  74.0  0.0        1
    ~~~

    <br>

  * If we wanna apply just one well-defined aggregation method to all columns or specific columns, we can directly use that method instead of agg (**groups.well-defined method()**)

  <br>

  ~~~python
  import pandas as pd
  import seaborn as sns

  titanic = sns.load_dataset('titanic')

  df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

  groups = df.groupby(['pclass'])

  mean_group = groups.mean()
  std_group = groups.std()
  mean_age_group = groups.age.mean()

  print(mean_group.head())
  print()
  print(std_group.head())
  print()
  print(mean_age_group)
  ~~~

  <br>

  ~~~
                age       fare  survived
  pclass
  1       38.233441  84.154687  0.629630
  2       29.877630  20.662183  0.472826
  3       25.140620  13.675550  0.242363

                age       fare  survived
  pclass
  1       14.802856  78.380373  0.484026
  2       14.001077  13.417399  0.500623
  3       12.495398  11.778142  0.428949

  pclass
  1    38.233441
  2    29.877630
  3    25.140620
  Name: age, dtype: float64
  ~~~

  <br>

### transform
* The methods for data aggregation or data change are applied to each group (agg, **transform**, apply, filter)
* **transform** method can be used only for **method which should be applied to the individual elements of dataframe** (not aggregation method)
  * **groups.transform(method)**
  * The result dataframe's index : orginal dataframe index (**break the group**)
  * If there are some column series that cannot be applied by method (ex. type is different), it ignores them and automatically calculate only desired columns
  * **groups.specific column.transform(method)**
    * The result is Series representing the result from mapping applied to only specific column

  <br>

  ~~~python
  import pandas as pd
  import seaborn as sns

  def func(x):                
      return (x - x.mean()) / x.std()         # Even if df is calculated with series, it is automatically calculated with common columns
                                              # z-score
  titanic = sns.load_dataset('titanic')

  df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

  groups = df.groupby(['pclass'])


  resDF1 = groups.transform(func)
  resDF2 = groups.age.transform(func)


  print(resDF1)
  print()
  print(resDF2)
  ~~~

  <br>

  ~~~
            age      fare  survived
  0   -0.251342 -0.545549 -0.565014
  1   -0.015770 -0.164217  0.765188
  2    0.068776 -0.488239  1.766263
  3   -0.218434 -0.396205  0.765188
  4    0.789041 -0.477626 -0.565014
  ..        ...       ...       ...
  886 -0.205529 -0.571063 -0.944475
  887 -1.299306 -0.690922  0.765188
  888       NaN  0.829880 -0.565014
  889 -0.826424 -0.690922  0.765188
  890  0.548953 -0.503097 -0.565014

  [891 rows x 3 columns]

  0     -0.251342
  1     -0.015770
  2      0.068776
  3     -0.218434
  4      0.789041
  ...
  886   -0.205529
  887   -1.299306
  888         NaN
  889   -0.826424
  890    0.548953
  Name: age, Length: 891, dtype: float64
  ~~~

<br>

### apply
* The methods for data aggregation or data change are applied to each group (agg, transform, **apply**, filter)
* **apply** method can be used for both **aggregation method** and **method which should be applied to the individual elements of dataframe**
* agg VS apply (with aggregation method)
  * **groups.agg(method)** == **groups.apply(method)** (except NaN, NaN == NaN : False)
     * If there are some column series that cannot be applied by method (ex. type is different), apply method cannot ignore them and there gonna be error

  <br>

  * **groups.specific_column.agg(method)** == **groups.specific_column.apply(method)**

  <br>

  * **groups.agg(lambda g : command using g.specific_column)** != **groups.apply(lambda g : command using g.specific_column)**
    * groups.agg(lambda g : command using g.specific_column) → represent all columns but all of column series are same (It is the result from applying method to specific column) → undesired
    * groups.apply(lambda g : command using g.specific_column) → represent only specific column

    ~~~python
    import pandas as pd
    import seaborn as sns

    titanic = sns.load_dataset('titanic')

    df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

    groups = df.groupby(['pclass'])


    resDF1 = groups.agg(lambda g : g.age.max())
    resDF2 = groups.apply(lambda g : g.age.max())
    resDF3 = groups.age.apply(max)


    print(resDF1)
    print()
    print(resDF2)
    print()
    print(resDF3)
    ~~~

    <br>

    ~~~
             age   sex  fare  survived  embark_town
    pclass
    1       80.0  80.0  80.0      80.0         80.0
    2       70.0  70.0  70.0      70.0         70.0
    3       74.0  74.0  74.0      74.0         74.0

    pclass
    1    80.0
    2    70.0
    3    74.0
    dtype: float64

    pclass
    1    80.0
    2    70.0
    3    74.0
    Name: age, dtype: float64

    ~~~

  <br>

* transform VS apply (with non-aggregation method)
  * **groups.specific_column.transform(method)** == **groups.specific_column.apply(method)**
    * Both yield the mapping result with broken group

  <br>

  * **groups.transform(lambda g : command using g.specific_column)** != **groups.apply(lambda g : command using g.specific_column)**
    * groups.transform(lambda g : command using g.specific_column) → **error**
    * groups.apply(lambda g : command using g.specific_column) == groups.specific_column.apply(method)

  <br>

  * **groups.transform(method)** == **groups.apply(method)**
    * If there are some column series that cannot be applied by method (ex. type is different), apply method cannot ignore them and there gonna be error
    * If there are multiple columns that are applied by method, apply method returns the dataframe with multi-index (first index : key, second index : original record index)

### Filter
* The methods for data aggregation or data change are applied to each group (agg, transform, apply, **filter**)
* Use boolean method and extract the groups that are True from the method → Make dataframe with original dataframe's order (break the groups)
* **groups.filter(boolean method)**

<br>

~~~python
import pandas as pd
import seaborn as sns

titanic = sns.load_dataset('titanic')

df = titanic.loc[:, ['pclass', 'age', 'sex', 'fare', 'survived', 'embark_town']]

groups = df.groupby(['pclass'])


resDF = groups.filter(lambda g : g.survived.sum() > 100)

print(resDF)

~~~

<br>

~~~
     pclass   age     sex     fare  survived  embark_town
0         3  22.0    male   7.2500         0  Southampton
1         1  38.0  female  71.2833         1    Cherbourg
2         3  26.0  female   7.9250         1  Southampton
3         1  35.0  female  53.1000         1  Southampton
4         3  35.0    male   8.0500         0  Southampton
..      ...   ...     ...      ...       ...          ...
885       3  39.0  female  29.1250         0   Queenstown
887       1  19.0  female  30.0000         1  Southampton
888       3   NaN  female  23.4500         0  Southampton
889       1  26.0    male  30.0000         1    Cherbourg
890       3  32.0    male   7.7500         0   Queenstown

[707 rows x 6 columns]
~~~

<br>

* Group2 (pclass == 2) is removed
