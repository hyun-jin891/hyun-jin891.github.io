---
layout: post
title: Visualization (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Visualization (pandas)
* Visualization of data using the tools of pandas
* If we wanna make more complex graph, use seaborn & matplotlib
* Use **df.plot()**

### Kind
* df.plot(kind='line') : line graph
* df.plot(kind='bar') : bar graph
* df.plot(kind='barh') : horizontal bar graph
* df.plot(kind='his') : histogram
* df.plot(kind='box') : box plot
* df.plot(kind='kde') : kernel density graph
* df.plot(kind='area') : area graph
* df.plot(kind='pie') : pie graph
* df.plot(kind='scatter') : scatter graph
* df.plot(kind='hexbin') : hexbin plot

### Line, Bar Graph
* **index of dataframe** → x axis
* **value of dataframe** → y axis
* **column of dataframe** → graph name (number of columns = number of graph)
* Line
  * **df.plot()** or **df.plot(kind='line')**
* Bar
  * **df.plot(kind='bar')**

### Histogram
* **index of dataframe** → element participating in counting frequency
* **value of dataframe** → x axis
* **column of dataframe** → graph name (number of columns = number of graph)
* **df.plot(kind='hist')**

### Scatter Graph
* One column of dataframe : x axis title
* another column of dataframe : y axis title
* **df.plot(x='c1', y='c2', kind='scatter')**

### Box Plot
* **df.plot(kind='box')**
  * Box plot for each column (column name = x axis)
  * value of dataframe → y axis

### Extra Knowledge
* wanna change dtypes of indexes or columns (usually change number object to numerical dtypes)
  * **df.index = df.index.map(A dtypes)**
    * dtypes of index changes to A dtypes
    * example: df.index = df.index.map(int)
  * **df.columns = df.columns.map(A dtypes)**
    * dtypes of column changes to A dtypes
