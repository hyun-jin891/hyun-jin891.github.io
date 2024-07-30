---
layout: post
title: Machine Learning Basic Flow with sklearn (Supervised Learning)
description: >
  Machine Learning
tags: [machine learning]
use_math: true
categories:
  - study
  - computer science
---
### Machine Learning Basic Flow with sklearn (Supervised Learning)
* Basic flow with sklearn for ML
* Examples are "Supervised Learning"

### Load Popular Dataset
* sklearn supports for us to load example datasets

<br>

~~~python
from sklearn.datasets import #
~~~

* load_iris()
* load_breast_cancer()
* ...
* feature
  * == character
  * ex) In breast cancer, 'mean radius' or 'mean texture' can be the features
* The datasets are stored as "Dictionary" (Supervised Learning)
  * data (key): data per each feature (ndarray)
  * feature_names (key): feature's name (list)
  * target (key): label value (for "Classification") or result number value (for "Regression") per each record (ndarray)
  * target_names (key): target's label name (ndarray)

  ~~~python
  from sklearn.datasets import load_breast_cancer
  import pandas as pd

  bc_dataset = load_breast_cancer()

  print(bc_dataset['data'])
  print('------------')
  print(bc_dataset['feature_names'])
  print('------------')
  print(bc_dataset['target'])
  print('------------')
  print(bc_dataset['target_names'])
  print('------------')

  df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
  df['clf_label'] = bc_dataset['target']

  print(df)

  ~~~

  ~~~
  [[1.799e+01 1.038e+01 1.228e+02 ... 2.654e-01 4.601e-01 1.189e-01]
  [2.057e+01 1.777e+01 1.329e+02 ... 1.860e-01 2.750e-01 8.902e-02]
  [1.969e+01 2.125e+01 1.300e+02 ... 2.430e-01 3.613e-01 8.758e-02]
  ...
  [1.660e+01 2.808e+01 1.083e+02 ... 1.418e-01 2.218e-01 7.820e-02]
  [2.060e+01 2.933e+01 1.401e+02 ... 2.650e-01 4.087e-01 1.240e-01]
  [7.760e+00 2.454e+01 4.792e+01 ... 0.000e+00 2.871e-01 7.039e-02]]
  ------------
  ['mean radius' 'mean texture' 'mean perimeter' 'mean area'
  'mean smoothness' 'mean compactness' 'mean concavity'
  'mean concave points' 'mean symmetry' 'mean fractal dimension'
  'radius error' 'texture error' 'perimeter error' 'area error'
  'smoothness error' 'compactness error' 'concavity error'
  'concave points error' 'symmetry error' 'fractal dimension error'
  'worst radius' 'worst texture' 'worst perimeter' 'worst area'
  'worst smoothness' 'worst compactness' 'worst concavity'
  'worst concave points' 'worst symmetry' 'worst fractal dimension']
  ------------
  [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
  1 0 0 0 0 0 0 0 0 1 0 1 1 1 1 1 0 0 1 0 0 1 1 1 1 0 1 0 0 1 1 1 1 0 1 0 0
  1 0 1 0 0 1 1 1 0 0 1 0 0 0 1 1 1 0 1 1 0 0 1 1 1 0 0 1 1 1 1 0 1 1 0 1 1
  1 1 1 1 1 1 0 0 0 1 0 0 1 1 1 0 0 1 0 1 0 0 1 0 0 1 1 0 1 1 0 1 1 1 1 0 1
  1 1 1 1 1 1 1 1 0 1 1 1 1 0 0 1 0 1 1 0 0 1 1 0 0 1 1 1 1 0 1 1 0 0 0 1 0
  1 0 1 1 1 0 1 1 0 0 1 0 0 0 0 1 0 0 0 1 0 1 0 1 1 0 1 0 0 0 0 1 1 0 0 1 1
  1 0 1 1 1 1 1 0 0 1 1 0 1 1 0 0 1 0 1 1 1 1 0 1 1 1 1 1 0 1 0 0 0 0 0 0 0
  0 0 0 0 0 0 0 1 1 1 1 1 1 0 1 0 1 1 0 1 1 0 1 0 0 1 1 1 1 1 1 1 1 1 1 1 1
  1 0 1 1 0 1 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 0 1 0 1 1 1 1 0 0 0 1 1
  1 1 0 1 0 1 0 1 1 1 0 1 1 1 1 1 1 1 0 0 0 1 1 1 1 1 1 1 1 1 1 1 0 0 1 0 0
  0 1 0 0 1 1 1 1 1 0 1 1 1 1 1 0 1 1 1 0 1 1 0 0 1 1 1 1 1 1 0 1 1 1 1 1 1
  1 0 1 1 1 1 1 0 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 0 1 0 0 1 0 1 1 1 1 1 0 1 1
  0 1 0 1 1 0 1 0 1 1 1 1 1 1 1 1 0 0 1 1 1 1 1 1 0 1 1 1 1 1 1 1 1 1 1 0 1
  1 1 1 1 1 1 0 1 0 1 1 0 1 1 1 1 1 0 0 1 0 1 0 1 1 1 1 1 0 1 1 0 1 0 1 0 0
  1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 0 1 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
  1 1 1 1 1 1 1 0 0 0 0 0 0 1]
  ------------
  ['malignant' 'benign']
  ------------
          mean radius  mean texture  mean perimeter  mean area  ...  worst concave points  worst symmetry  worst fractal dimension  clf_label
     0          17.99         10.38          122.80     1001.0  ...                0.2654          0.4601                  0.11890          0
     1          20.57         17.77          132.90     1326.0  ...                0.1860          0.2750                  0.08902          0
     2          19.69         21.25          130.00     1203.0  ...                0.2430          0.3613                  0.08758          0
     3          11.42         20.38           77.58      386.1  ...                0.2575          0.6638                  0.17300          0
     4          20.29         14.34          135.10     1297.0  ...                0.1625          0.2364                  0.07678          0
     ..           ...           ...             ...        ...  ...                   ...             ...                      ...        ...
     564        21.56         22.39          142.00     1479.0  ...                0.2216          0.2060                  0.07115          0
     565        20.13         28.25          131.20     1261.0  ...                0.1628          0.2572                  0.06637          0
     566        16.60         28.08          108.30      858.1  ...                0.1418          0.2218                  0.07820          0
     567        20.60         29.33          140.10     1265.0  ...                0.2650          0.4087                  0.12400          0
     568         7.76         24.54           47.92      181.0  ...                0.0000          0.2871                  0.07039          1

     [569 rows x 31 columns]
  ~~~

## Select Train & Test Data from Dataset
* If we have a dataset (like dataframe), there may be many records
* We have to select the records playing a role in train_data and the records playing a role in test_data
* **train_test_split(data, test_size=, random_state=, stratify=y)**

  ~~~python
  from sklearn.model_selection import train_test_split
  ~~~

  * For the case of supervised learning, data have to split to 2 variables (x: independent variable, y: dependent variable)
  * test_size = α -> test_data_size = α, train_data_size = 1 - α
  * random_state = n (value is not important): Even if we try to call this function for multiple times, it will return same result
  * stratify=(dependent variable): sample distribution ≒ population distribution (It is useful for "Classification")

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
import pandas as pd

bc_dataset = load_breast_cancer()

df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
df['clf_label'] = bc_dataset['target']

df_x = df.iloc[:, :-1]
df_y = df['clf_label']

X_train, X_test, Y_train, Y_test = train_test_split(df_x, df_y, test_size=0.2, random_state=1, stratify=df_y)

~~~

## Supervised Learning Algorithm Overview
* Estimator
  * Super class for all supervised learning algorithm
  * It has 2 child class: Classifier (for "Classification") & Regressor (for "Regression")
  * Estimator has 2 major methods: **fit()** (training), **predict()** (prediction for result)
    * In unsupervised learning, their meaning is different
      * fit(): preprocessing
      * transform(): actual process
* Classifier
  * class for "Classification"
  * "Classification"
    * We have to make the model predicting y (dependent variable) from x (independent variable)
    * If y is labeling data, it is called "Classification"
  * Type
    * DecisionTreeClassifier
    * GradientBoostingClassifier
    * GaussianNB
    * SVC
    * RandomForestClassifier
* Regressor
  * class for "Regressor"
  * "Regression"
    * We have to make the model predicting y (dependent variable) from x (independent variable)
    * If y is not labeling data, it is called "Regression"
    * Type
      * LinearRegression
      * Lasso
      * GradientBoostingRegressor
      * Ridge
      * RandomForestRegressor

## Training and Predict with ML Class' Object
* Formation of ML class' object → fit (training with X_train, Y_train) → predict (with X_test)

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
import pandas as pd

bc_dataset = load_breast_cancer()

df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
df['clf_label'] = bc_dataset['target']

df_x = df.iloc[:, :-1]
df_y = df['clf_label']

X_train, X_test, Y_train, Y_test = train_test_split(df_x, df_y, test_size=0.2, random_state=1, stratify=df_y)

clf_model = DecisionTreeClassifier(random_state=1)
clf_model.fit(X_train, Y_train)
res = clf_model.predict(X_test)
~~~

## Evaluation with Prediction and Y_test
* Comparison between prediction from the model and Y_test
* There are several ways for evaluation
  * example: accuracy_score

  ~~~python
  from sklearn.metrics import accuracy_score
  ~~~

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score
import numpy as np
import pandas as pd

bc_dataset = load_breast_cancer()

df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
df['clf_label'] = bc_dataset['target']

df_x = df.iloc[:, :-1]
df_y = df['clf_label']

X_train, X_test, Y_train, Y_test = train_test_split(df_x, df_y, test_size=0.2, random_state=1, stratify=df_y)

clf_model = DecisionTreeClassifier(random_state=1)
clf_model.fit(X_train, Y_train)
res = clf_model.predict(X_test)

print(np.round(accuracy_score(Y_test, res), 2))
~~~

~~~
0.96
~~~

## Basic Flow for Supervised Learning
* 1<sup>st</sup>: Get Dataset
* 2<sup>nd</sup>: Preprocessing of Dataset → Split x and y
* 3<sup>rd</sup>: Split X_train, X_test, Y_train, Y_test
* 4<sup>th</sup>: Formation of ML class' object → fit (training with X_train, Y_train) → predict (with X_test)
* 5<sup>th</sup>: Evaluation (Comparison between prediction from the model and Y_test)
