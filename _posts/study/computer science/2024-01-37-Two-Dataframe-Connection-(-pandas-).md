---
layout: post
title: Two Dataframe Connection (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Two Dataframe Connection (pandas)
* **concat**
  * simple connection between 2 dataframe
* **merge**
  * connection standard : common data on specific column → combine those records (df1 has n records & df2 has m records that have common data on specific column compared with counterpart df → Number of total intersection records is n x m)
* **join**
  * connection standard : common row index
* Option used for 3 methods
  * **inner** : intersection set
  * **outer** : union set (contains intersection set)
  * **left** : left set (contains intersection set)
  * **right** : right set (contains intersection set)

### Concat
* **pd.concat()**
* simple connection between 2 dataframe
* axis=0
  * If same name exists in both df's column list, combine them (Result column list consists of left df column, intersection column, right df column)
  * Each record's name(index) remains (just connection)
  * join='outer' : return result dataframe (left + intersection + right) from Concat method
  * join='inner' : return result dataframe which contains only intersection columns
  * ignore_index=True : original index removed + set again as integer index
  <br>

  ~~~python
  import pandas as pd

  df1 = pd.DataFrame({'c1' : [30, 21, 30, 40], 'c2' : [40, 50, 50, 20], 'c3' : [70, 50, 60, 17]}, index=[0, 1, 2, 3])

  df2 = pd.DataFrame({'c1' : [15, 46, 78, 100], 'c4' : [54, 15, 26, 49], 'c3' : [55, 79, 44, 10], 'c9' : [11, 56, 74, 8]}, index=[2, 3, 4, 5])

  df3 = pd.concat([df1, df2], axis=0, join='outer')
  df4 = pd.concat([df2, df1], axis=0, join='outer')
  df5 = pd.concat([df1, df2], axis=0, ignore_index=True)
  df6 = pd.concat([df1, df2], axis=0, join='inner')

  print(df1)
  print()
  print(df2)
  print()
  print(df3)
  print()
  print(df4)
  print()
  print(df5)
  print()
  print(df6)
  ~~~

  <br>

  ~~~
     c1  c2  c3
  0  30  40  70
  1  21  50  50
  2  30  50  60
  3  40  20  17

      c1  c4  c3  c9
  2   15  54  55  11
  3   46  15  79  56
  4   78  26  44  74
  5  100  49  10   8

      c1    c2  c3    c4    c9
  0   30  40.0  70   NaN   NaN
  1   21  50.0  50   NaN   NaN
  2   30  50.0  60   NaN   NaN
  3   40  20.0  17   NaN   NaN
  2   15   NaN  55  54.0  11.0
  3   46   NaN  79  15.0  56.0
  4   78   NaN  44  26.0  74.0
  5  100   NaN  10  49.0   8.0

      c1    c4  c3    c9    c2
  2   15  54.0  55  11.0   NaN
  3   46  15.0  79  56.0   NaN
  4   78  26.0  44  74.0   NaN
  5  100  49.0  10   8.0   NaN
  0   30   NaN  70   NaN  40.0
  1   21   NaN  50   NaN  50.0
  2   30   NaN  60   NaN  50.0
  3   40   NaN  17   NaN  20.0

      c1    c2  c3    c4    c9
  0   30  40.0  70   NaN   NaN
  1   21  50.0  50   NaN   NaN
  2   30  50.0  60   NaN   NaN
  3   40  20.0  17   NaN   NaN
  4   15   NaN  55  54.0  11.0
  5   46   NaN  79  15.0  56.0
  6   78   NaN  44  26.0  74.0
  7  100   NaN  10  49.0   8.0

      c1  c3
  0   30  70
  1   21  50
  2   30  60
  3   40  17
  2   15  55
  3   46  79
  4   78  44
  5  100  10
  ~~~

  <br>

* axis=1
  * If same name exists in both df's index list, combine them (Result index list consists of left df index, intersection index, right df index)
  * Each record's name(index) remains (just connection)
  * join='outer' : return result dataframe (left + intersection + right) from Concat method
  * join='inner' : return result dataframe which contains only intersection index
  * ignore_index=True : original index removed + set again as integer index

  <br>

  ~~~python
  import pandas as pd

  df1 = pd.DataFrame({'c1' : [30, 21, 30, 40], 'c2' : [40, 50, 50, 20], 'c3' : [70, 50, 60, 17]}, index=[0, 1, 2, 3])

  df2 = pd.DataFrame({'c1' : [15, 46, 78, 100], 'c4' : [54, 15, 26, 49], 'c3' : [55, 79, 44, 10], 'c9' : [11, 56, 74, 8]}, index=[2, 3, 4, 5])

  df7 = pd.concat([df1, df2], axis=1, join='outer')
  df8 = pd.concat([df2, df1], axis=1, join='outer')
  df9 = pd.concat([df1, df2], axis=1, join='inner')

  print(df7)
  print()
  print(df8)
  print()
  print(df9)
  ~~~

  <br>


  ~~~
       c1    c2    c3     c1    c4    c3    c9
  0  30.0  40.0  70.0    NaN   NaN   NaN   NaN
  1  21.0  50.0  50.0    NaN   NaN   NaN   NaN
  2  30.0  50.0  60.0   15.0  54.0  55.0  11.0
  3  40.0  20.0  17.0   46.0  15.0  79.0  56.0
  4   NaN   NaN   NaN   78.0  26.0  44.0  74.0
  5   NaN   NaN   NaN  100.0  49.0  10.0   8.0

        c1    c4    c3    c9    c1    c2    c3
  0    NaN   NaN   NaN   NaN  30.0  40.0  70.0
  1    NaN   NaN   NaN   NaN  21.0  50.0  50.0
  2   15.0  54.0  55.0  11.0  30.0  50.0  60.0
  3   46.0  15.0  79.0  56.0  40.0  20.0  17.0
  4   78.0  26.0  44.0  74.0   NaN   NaN   NaN
  5  100.0  49.0  10.0   8.0   NaN   NaN   NaN

     c1  c2  c3  c1  c4  c3  c9
  2  30  50  60  15  54  55  11
  3  40  20  17  46  15  79  56
  ~~~

<br>

* We can make connection between dataframe and series or between series and series

<br>

~~~python
import pandas as pd

df1 = pd.DataFrame({'c1' : [30, 21, 30, 40], 'c2' : [40, 50, 50, 20], 'c3' : [70, 50, 60, 17]}, index=[0, 1, 2, 3])

s1 = pd.Series([45, 100], name='c10', index=[2, 3])
s2 = pd.Series([50, 11], name='c11', index=[2, 4])

df10 = pd.concat([df1, s1], axis=1)
df11 = pd.concat([s1, s2], axis=1)
s3 = pd.concat([s1, s2], axis=0)

print(df10)
print()
print(df11)
print()
print(s3)
~~~

~~~
   c1  c2  c3    c10
0  30  40  70    NaN
1  21  50  50    NaN
2  30  50  60   45.0
3  40  20  17  100.0

     c10   c11
2   45.0  50.0
3  100.0   NaN
4    NaN  11.0

2     45
3    100
2     50
4     11
dtype: int64
~~~

### Merge
* **pd.merge()**
* connection standard : common data on specific column → combine those records (df1 has n records & df2 has m records that have common data on specific column compared with counterpart df → Number of total intersection records is n x m)

<br>

~~~python
import pandas as pd


df1 = pd.DataFrame({'math' : [30, 21, 30, 40], 'english' : [40, 50, 50, 20], 'biology2' : [70, 50, 60, 17]}, index=[0, 1, 2, 3])


df2 = pd.DataFrame({'math' : [15, 30, 78, 30], 'biology1' : [50, 17, 26, 49], 'physics' : [55, 79, 44, 10], 'english' : [11, 50, 74, 8]}, index=[2, 3, 4, 5])


df3 = pd.merge(df1, df2, how='outer', on='math')
df4 = pd.merge(df1, df2, how='inner', on='math')
df5 = pd.merge(df2, df1, how='outer', on='math')

print(df1)
print()
print(df2)
print()
print(df3)
print()
print(df4)
print()
print(df5)
~~~

<br>

~~~
   math  english  biology2
0    30       40        70
1    21       50        50
2    30       50        60
3    40       20        17

   math  biology1  physics  english
2    15        50       55       11
3    30        17       79       50
4    78        26       44       74
5    30        49       10        8

   math  english_x  biology2  biology1  physics  english_y
0    30       40.0      70.0      17.0     79.0       50.0
1    30       40.0      70.0      49.0     10.0        8.0
2    30       50.0      60.0      17.0     79.0       50.0
3    30       50.0      60.0      49.0     10.0        8.0
4    21       50.0      50.0       NaN      NaN        NaN
5    40       20.0      17.0       NaN      NaN        NaN
6    15        NaN       NaN      50.0     55.0       11.0
7    78        NaN       NaN      26.0     44.0       74.0

   math  english_x  biology2  biology1  physics  english_y
0    30         40        70        17       79         50
1    30         40        70        49       10          8
2    30         50        60        17       79         50
3    30         50        60        49       10          8

   math  biology1  physics  english_x  english_y  biology2
0    15      50.0     55.0       11.0        NaN       NaN
1    30      17.0     79.0       50.0       40.0      70.0
2    30      17.0     79.0       50.0       50.0      60.0
3    30      49.0     10.0        8.0       40.0      70.0
4    30      49.0     10.0        8.0       50.0      60.0
5    78      26.0     44.0       74.0        NaN       NaN
6    21       NaN      NaN        NaN       50.0      50.0
7    40       NaN      NaN        NaN       20.0      17.0
~~~

<br>

* df3 = pd.merge(df1, df2, how='outer', on='math')
  * on='math' : connect the records that have same data on 'math' column (df1 has 2 records and df2 has 2 records that have 30 as data on 'math' → Consequently, the number of intersection records is 4)
  * how='outer' : return result dataframe (left + intersection + right)
* df4 = pd.merge(df1, df2, how='inner', on='math')
  * how='inner' : return result dataframe (only intersection records)

<br>

~~~python
import pandas as pd


df1 = pd.DataFrame({'math' : [30, 21, 30, 40], 'english' : [40, 50, 50, 20], 'biology2' : [70, 50, 60, 17]}, index=[0, 1, 2, 3])


df2 = pd.DataFrame({'math' : [15, 30, 78, 30], 'biology1' : [50, 17, 26, 49], 'physics' : [55, 79, 44, 10], 'english' : [11, 50, 74, 8]}, index=[2, 3, 4, 5])


df6 = pd.merge(df2, df1, how='outer', on=['math', 'english'])

print(df6)

~~~

<br>

~~~
   math  biology1  physics  english  biology2
0    15      50.0     55.0       11       NaN
1    30      17.0     79.0       50      60.0
2    78      26.0     44.0       74       NaN
3    30      49.0     10.0        8       NaN
4    30       NaN      NaN       40      70.0
5    21       NaN      NaN       50      50.0
6    40       NaN      NaN       20      17.0
~~~

<br>

* df6 = pd.merge(df2, df1, how='outer', on=['math', 'english'])
  * on=['math', 'english'] : connect the records that have same data on both 'math' column and 'english' column (df1 has 1 record and df2 has 1 record that have (30, 50) as data on 'math' and 'english' → Consequently, the number of intersection records is 1)

<br>

~~~python
import pandas as pd


df1 = pd.DataFrame({'math' : [30, 21, 30, 40], 'english' : [40, 50, 50, 20], 'biology2' : [70, 50, 60, 17]}, index=[0, 1, 2, 3])


df2 = pd.DataFrame({'math' : [15, 30, 78, 30], 'biology1' : [50, 17, 26, 49], 'physics' : [55, 79, 44, 10], 'english' : [11, 50, 74, 8]}, index=[2, 3, 4, 5])

df7 = pd.merge(df1, df2, how='left', on='math')
df8 = pd.merge(df1, df2, how='right', on='math')

print(df7)
print()
print(df8)
~~~

<br>

~~~
   math  english_x  biology2  biology1  physics  english_y
0    30         40        70      17.0     79.0       50.0
1    30         40        70      49.0     10.0        8.0
2    21         50        50       NaN      NaN        NaN
3    30         50        60      17.0     79.0       50.0
4    30         50        60      49.0     10.0        8.0
5    40         20        17       NaN      NaN        NaN

   math  english_x  biology2  biology1  physics  english_y
0    15        NaN       NaN        50       55         11
1    30       40.0      70.0        17       79         50
2    30       50.0      60.0        17       79         50
3    78        NaN       NaN        26       44         74
4    30       40.0      70.0        49       10          8
5    30       50.0      60.0        49       10          8
~~~

<br>

* df7 = pd.merge(df1, df2, how='left', on='math')
  * how='left' : return result dataframe (left + intersection)
* df8 = pd.merge(df1, df2, how='right', on='math')
  * how='right' : return result dataframe (intersection + right)

<br>

~~~python
import pandas as pd


df1 = pd.DataFrame({'math' : [30, 21, 30, 40], 'english' : [40, 50, 50, 20], 'biology2' : [70, 50, 60, 17]}, index=[0, 1, 2, 3])


df2 = pd.DataFrame({'math' : [15, 30, 78, 30], 'biology1' : [50, 17, 26, 49], 'physics' : [55, 79, 44, 10], 'english' : [11, 50, 74, 8]}, index=[2, 3, 4, 5])

df9 = pd.merge(df1, df2, how='outer', left_on='biology2', right_on='biology1')

print(df9)
~~~

<br>

~~~
   math_x  english_x  biology2  math_y  biology1  physics  english_y
0    30.0       40.0      70.0     NaN       NaN      NaN        NaN
1    21.0       50.0      50.0    15.0      50.0     55.0       11.0
2    30.0       50.0      60.0     NaN       NaN      NaN        NaN
3    40.0       20.0      17.0    30.0      17.0     79.0       50.0
4     NaN        NaN       NaN    78.0      26.0     44.0       74.0
5     NaN        NaN       NaN    30.0      49.0     10.0        8.0
~~~


<br>

* df9 = pd.merge(df1, df2, how='outer', left_on='biology2', right_on='biology1')
  * left_on='biology2', right_on='biology1' : connect the records that df1 value from biology2 is equal to df2 value from biology1  (df1 has 1 record and df2 has 1 record that have 50 as data on 'biology2' and 'biology1' → Consequently, the number of intersection records is 1) + (df1 has 1 record and df2 has 1 record that have 17 as data on 'biology2' and 'biology1' → Consequently, the number of intersection records is 1 )

### Join
* **df1.join(df2)**
* Similar with merge, but the connection standard is common row index (There should not be common name on 2 column lists)

<br>

~~~python
import pandas as pd

df1 = pd.DataFrame({'math1' : [30, 21, 30, 40], 'english1' : [40, 50, 50, 20], 'biology2' : [70, 50, 60, 17]}, index=["Cho", "Kim", "Rivera", "Samoh"])


df2 = pd.DataFrame({'math2' : [15, 30, 78, 30], 'biology1' : [50, 17, 26, 49], 'physics' : [55, 79, 44, 10], 'english2' : [11, 50, 74, 8]}, index=["Sanford", "Cho", "Samoh", "Dongmo"])

df3 = df1.join(df2, how='outer')
df4 = df1.join(df2, how='inner')

print(df1)
print()
print(df2)
print()
print(df3)
print()
print(df4)

~~~

<br>

~~~
        math1  english1  biology2
Cho        30        40        70
Kim        21        50        50
Rivera     30        50        60
Samoh      40        20        17

         math2  biology1  physics  english2
Sanford     15        50       55        11
Cho         30        17       79        50
Samoh       78        26       44        74
Dongmo      30        49       10         8

         math1  english1  biology2  math2  biology1  physics  english2
Cho       30.0      40.0      70.0   30.0      17.0     79.0      50.0
Dongmo     NaN       NaN       NaN   30.0      49.0     10.0       8.0
Kim       21.0      50.0      50.0    NaN       NaN      NaN       NaN
Rivera    30.0      50.0      60.0    NaN       NaN      NaN       NaN
Samoh     40.0      20.0      17.0   78.0      26.0     44.0      74.0
Sanford    NaN       NaN       NaN   15.0      50.0     55.0      11.0

       math1  english1  biology2  math2  biology1  physics  english2
Cho       30        40        70     30        17       79        50
Samoh     40        20        17     78        26       44        74
~~~

<br>

* df3 = df1.join(df2, how='outer')
  * Common indexes are 'Cho' and 'Samoh', so it connects between 'Cho' record from df1 and 'Cho' record from df2 + between 'Samoh' record from df1 and 'Samoh' record from df2
  * how='outer' : return result dataframe (left + intersection + right)
* df4 = df1.join(df2, how='inner')
  * Common indexes are 'Cho' and 'Samoh', so it connects between 'Cho' record from df1 and 'Cho' record from df2 + between 'Samoh' record from df1 and 'Samoh' record from df2
  * how='inner' : return result dataframe (only intersection records)
