---
layout: post
title: Missed Data (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Missed Data (pandas)
* Some elements in dataframe can be missed
* They are usually expressed as **NaN** (np.nan)
  * If they are not NaN, it is better to change them to NaN
    * **df = df.replace('them', np.nan)**
* On data analysis
  * 1<sup>st</sup> check whether there are missed data
  * 2<sup>nd</sup> remove or replace them

### Check Missed Data
* method 1 : **df.info()**
  * We can get the information about the number of non-null data of each column
* method 2 : **df[column's name].value_counts(dropna=False)**
  * We can get the number of NaN data directly because its result is Series about the number of unique values in a specific column (If the option of dropna is False, NaN is also unique value)
* method 3 : **df.isnull()**
  * We can get the dataframe whose elements have boolean value (If the location has NaN, the element on it is True)
* method 4 : **df.notnull()**
  * We can get the dataframe whose elements have boolean value (If the location has NaN, the element on it is False)
* method 5 : **df.sum(axis=0)**
  * We can get Series (index : column's name, value : sum of all elements from one column)
  * If we use this as **df.isnull().sum(axis=0)**, we can get Series that represents the number of NaN data for each column (because True is calculated as 1)

### Remove Missed Data
* If there are lots of missed data on specific row or column, remove the whole row or column
* **df = df.dropna(axis=1, thresh=n)**
  * If the column has NaN data >= n, the whole column is removed
  * df.dropna(axis=0, thresh=n) : If the row has NaN data >= n, the whole row is removed
* **df = df.dropna(subset=[column's name], axis=0)**
  * On a specifc column, rows that have NaN value are removed

### Replace Missed Data
* **df = df[column's name].fillna(value)** (can use inplace option)
  * On a specific column, NaN gonna be changed to 'value'
  * 'value' can be
    * mean : df[column's name].mean()
    * median : df[column's name].median()
    * Top : **df[column's name].value_counts(dropna=True).idxmax()** : return index's name that has maximum value
    * previous row's value : **df = df[column's name].fillna(method='ffill')**
    * next row's value : **df = df[column's name].fillna(method='bfill')**
