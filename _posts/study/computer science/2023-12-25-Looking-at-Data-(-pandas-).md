---
layout: post
title: Looking at Data (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Looking at Data (pandas)
* basic information about dataframe
* statistical information about dataframe (describe method)
  * numerical
    * count
    * mean
    * max
    * 75%
    * 50%
    * 25%
    * min
    * std
  * object (non-numerical)
    * count
    * top
    * unique value
    * freq
* preview of data (head & tail)
* the number of rows & columns
* the number of data
  * count
  * unique value's count
* Statistical method
  * mean
  * median
  * max
  * min
  * std
  * corr

### Basic Information
* **df.info()**
* result
  * class type
  * the number of indexes
  * the number of columns
  * Each column's information
    * column name
    * the number of data whose value is not NaN
    * data type (dtypes)
  * the number of each dtypes
    * example : int64, float64, object
    * If there is at least one object value, the column's dtypes is **object** however the left values are numerical data
  * memory usage

### Statistical Information
* **df.describe()** (only for numerical data type columns)
  * count : the number of data for each column
  * mean : mean for each column
  * std : std for each column
  * min : min for each column
  * 25% : value on 25% location for each column
  * 50% : value on 50% location for each column
  * 75% : value on 75% location for each column
  * max : max for each column  
* **df.describe(include='all')** (contains the object data type columns)
  * add 3 categories from the result of df.describe()
  * unique : the number of data considering repeating data
  * top : value (dtypes : object) having the highest frequency
  * freq : top's frequency

### Preview of Data
* **df.head(number)** (return dataframe with n rows starting from the first row, default : number == 5)
* **df.tail(number)** (return dataframe with n rows starting from the last row, default : number == 5)

### Number of Rows & Columns
* **df.shape**
* result : (row's number, column's number)

### Number of Data
* **df.count()**
  * return Series (the number of data for each column)
  * not count NaN
* **df[column's name].count()**
  * return count_value of a specific column
  * not count NaN
* **df[**[c1, c2, ...]**].count()**
  * return Series (count_value of c1, c2, ...)
  * not count NaN
* **df[column's name].value_counts(dropna=False)**
  * return Series (index : unique values from a specific column, value of Series : the number of each unique value from a specific column)
  * count NaN as well (if we don't wanna contain NaN, dropna=True)

### Statistical Method
* mean
  * **df.mean()**
    * return Series (mean for each column)
  * **df[column's name].mean()**
    * return mean_value of a specific column
  * **df[**[c1, c2, ...]**].mean()**
    * return Series (mean of c1, c2, ...)
* median
  * **df.median()**
    * return Series (median for each column)
  * **df[column's name].median()**
    * return median_value of a specific column
  * **df[**[c1, c2, ...]**].median()**
    * return Series (median of c1, c2, ...)
* max
  * If dtypes is object, it compares the values based on ASCII
  * **df.max()**
    * return Series (max for each column)
  * **df[column's name].max()**
    * return max_value of a specific column
  * **df[**[c1, c2, ...]**].max()**
    * return Series (max of c1, c2, ...)
* min
  * If dtypes is object, it compares the values based on ASCII
  * **df.min()**
    * return Series (min for each column)
  * **df[column's name].min()**
    * return min_value of a specific column
  * **df[**[c1, c2, ...]**].min()**
    * return Series (min of c1, c2, ...)
* std
  * **df.std()**
    * return Series (std for each column)
  * **df[column's name].std()**
    * return std_value of a specific column
  * **df[**[c1, c2, ...]**].std()**
    * return Series (std of c1, c2, ...)
* corr (linear correlation coefficient)
  * abs(corr) → 1 : linear relationship
  * corr < 0 : negative relationship
  * corr > 0 : positive relationship
  * **df.corr()**
    * return dataframe (corr of all pair from all columns)
  * **df[**[c1, c2, ....]**].corr()**
    * return dataframe (corr of all pair from c1, c2, ....)
