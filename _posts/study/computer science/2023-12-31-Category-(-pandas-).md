---
layout: post
title: Category (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Category (pandas)
* Conversion from the continuous data
* One-hot-encoding

### Conversion From Continuous Data
* For conversion from the continuous data to category data type
  * 1<sup>st</sup> : divide the section with same width
    * **data_count, section = np.histogram(df[column's name], bins=n)**
    * Given a specific column's data, it calculates n sections and each section contains the data whose value is involved in that section's range
    * data_count : array that involves the number of data that each section contains
    * **section** : array that involves the boundaries that each section has
      * The number of its elements = n + 1 because they are boundaries
  * 2<sup>nd</sup> : assign the name which gonna be the value of category data to each section
    * **section_names = ["name1", "name2", ...]**
  * 3<sup>rd</sup> : Which section does a specific record (row) involve?
    * **df[new column's name] = pd.cut(x=df[column's name], bins=section, labels=section_names, include_lowest=True)**
    * If one datum's value from one record is involved in "name1" section's range (minimum **<=** x < maximum), the record's datum in new column is "name1"
    <br>

    ~~~
    <example>
         new column
    0      name2
    1      name1
    2      name1
    3      name3
    4      name2
    5      name1
    6      name3
    .
    .
    .
    ~~~

    <br>

### One-Hot-Encoding
* When we do machine learning, category data should be converted to dummy variable data (one-hot-encoding)
* Dummy variable : only consists of 0 and 1
  * 1 : It is involved in a specific category
  * 0 : It is not involved in a specific category
* Method 1 : pandas
  * Dummy variable form
    * Dataframe (index : each record index, column : category data' names)
    <br>

    ~~~
    <example>
        name1    name2    name3
    0     1        0        0  # 0 record is involved in name1 category
    1     0        0        1
    2     0        0        1
    3     0        1        0
    4     1        0        0
    5     1        0        0
    6     0        0        1
    .
    .
    .
    ~~~
    <br>
  * **dummies = pd.get_dummies(df[column's name])**
    * It yields the dummies from a specific column whose dtypes is category

* Method 2 : sklearn
  * **from sklearn import preprocessing**
  * Dummy variable form : sparse matrix (It has lots of 0 as data)
    * Sparse matrix is represented like this
      * (row number, column number)    1.0 : The value on this location of sparse matrix is 1.0
      <br>

      ~~~
      <example from previous dummies>

      (0, 0)        1.0
      (1, 2)        1.0
      (2, 2)        1.0
      (3, 1)        1.0
      (4, 0)        1.0
      (5, 0)        1.0
      (6, 2)        1.0
      .
      .
      .
      <class 'scipy.sparse.csr.csr_matrix'>
      # The value on left location is 0
      ~~~
      <br>

  * Code
    <br>
    ~~~python
    from sklearn import preprocessing

    labelEncoder = preprocessing.LabelEncoder()
    oneHotEncoder = preprocessing.OneHotEncoder()

    labeledOneHot = LabelEncoder.fit_transform(df['c1']) # c1 column is category data
    # This code's result is to return numpy.ndarray / Example : [0 2 2 1 0 0 2] (1 record is involved in "name3" category)
    # This array is 1D vector and it should be converted to n x 1 matrix

    newLabeledOneHot = labeledOneHot.reshape(len(labeledOneHot, 1)) # len(labeledOneHot) x 1 matrix

    oneHotResult = oneHotEncoder.fit_transform(newLabeledOneHot) # Sparse matrix
    ~~~
