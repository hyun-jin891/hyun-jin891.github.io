---
layout: post
title: Duplicated Data (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Duplicated Data (pandas)
* On data analysis
  * 1<sup>st</sup> : Check whether there are duplicated data
    * Standard can be various (ex. Values from all columns are same on two rows, Values from just 2 columns are same on two rows)
  * 2<sup>nd</sup> : remove the duplicated (corresponding to specific standard) row

### Check Duplicated Data
* **df.duplicated()**
  * Return Series that has boolean value
    * If one row (one index of Series) is duplicated from the previous row, the value of Series is True
* **df[column's name].duplicated()** or **df[**['c1', 'c2', ....]**].duplicated()**
  * Return Series that has boolean value
    * Standard for checking whether it is duplicated or not is limited to some columns

### Remove Duplicated Data
* **df = df.drop_duplicates()** (inplace can be used)
  * Dataframe that duplicated data (row) are removed
* **df = df.drop_duplicates(subset=['c1', 'c2', ...])**
  * Dataframe that duplicated data (row) checked using some columns as standard are removed
