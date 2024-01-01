---
layout: post
title: Normalization (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Normalization (pandas)
* If data range of a specific column is so wide, it can have influence on the result of machine learning more than other columns.
* We should make the data range from each column be 0 ~ 1 or -1 ~ 1

### Method
* 1: (a datum) / (maximum data of the column)
  * **df[column's name] = df[column's name] / df[column's name].max()**
* 2: (a datum - minimum data) / (maximum data - minimum data)
  * **df[column's name] = (df[column's name] - df[column's name].min()) / (df[column's name].max() - df[column's name].min())**
