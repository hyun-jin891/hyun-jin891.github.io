---
layout: post
title: Conversion in Column (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Conversion in Column (pandas)
* Conversion of unit
* Conversion of data type

### Unit Conversion
* We make the Unit Conversion Factors
  * ex) kg → g : 1000 g / 1 kg
* original data * (Unit Conversion Factor) → data with new unit
* **df[new column's name] = df[original column's name] * (Unit Conversion Factor)**
  * add a new column from an original column to the dataframe

### Data Type Conversion
* **df[column's name] = df[column's name].astype('new dtypes')**
* There are lots of cases requiring the conversion of data type
  * Example case 1 : When the numerical data in a specific column which should be processed as number are stored as object because of some non-numerical data in the same column
    * 1<sup>st</sup> : remove the rows having non-numerical data
    * 2<sup>nd</sup> : **df[column's name] = df[column's name].astype('float')** (or 'int')
    * If we wanna be back to object, **df[column's name] = df[column's name].astype('str')**
  * Example case 2 : **Category dtypes**
    * wanna make the groups (Group name is the data in a specific column)
    * **df[column's name] = df[column's name].astype("category")**

### Extra Knowledge
* round
  * **df[column's name] = df[column's name].round(n)** : At n location, it gonna happen
  * If n == 2, 1.879 → 1.9
* **df.dtypes**
  * return Series (index : names of columns, value : dtypes)
* **df[column's name].unique()**
  * return list for the unique values of a specific column
* **df[column's name].replace({original data1 : new data1, original data2 : new data2, ....})**
  * In a specific column, all of specific data (keys) are changed to "new data" (There gonna be automatically coversion of dtypes)
* **df[column's name].sample(n)**
  * return Series that has n data selected from the dataframe
