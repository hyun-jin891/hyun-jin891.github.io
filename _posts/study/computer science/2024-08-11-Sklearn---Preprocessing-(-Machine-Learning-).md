---
layout: post
title: Sklearn-Preprocessing (Machine Learning)
description: >
  Machine Learning
tags: [machine learning]
use_math: true
categories:
  - study
  - computer science
---
### Sklearn-Preprocessing (Machine Learning)
* Before using machine learning, we need to do preprocessing of the data
  * Null
  * Data Encoding
  * Feature Scaling

### Null
* If one feature has lots of Null, we need to drop it
* If one feature has a few Null data and it is important feature, we need to replace it to a specific value (like mean)
* If one feature has lots of Null but it is important feature, we need to discuss and select the value that gonna replace Null

### Data Encoding
* Machine learning cannot calculate the object (string) feature
  * Object feature == Category feature + Text feature
* We need to convert them to numerical data
  * It is called data encoding
* There are 2 ways for data encoding
  * Label Encoding
  * One-Hot Encoding
* Label Encoding
  * If a single feature has "A", "B", "C", "D" as data
  * "A" → 0, "B" → 1, "C" → 2, "D" → 3
  * It is easy to understand but there are problems of label encoding
    * If order of magnitude is important in certain ML algorithms like linear regression, it will yield low performance
    * Tree ML algorithms is ok to use label encoding

  ~~~python
  from sklearn.preprocessing import LabelEncoder

  alphabet_features = ["A", "B", "C", "D"]

  label_encoder = LabelEncoder()
  label_encoder.fit(alphabet_features)

  labels = label_encoder.transform(alphabet_features)

  print(labels)

  print("--------------")

  print(label_encoder.classes_)

  print("--------------")

  print(label_encoder.inverse_transform([2, 3, 0, 1]))

  ~~~

  ~~~
  [0 1 2 3]
  --------------
  ['A' 'B' 'C' 'D']
  --------------
  ['C' 'D' 'A' 'B']
  ~~~

* One-Hot Encoding
  * If there are 4 records and a single feature has "A", "B", "C", "D" as data

  ~~~
  Original dataframe

                             feature
                 record 1      "A"
                 record 2      "D"
                 record 3      "A"
                 record 4      "B"

                            ↓

              feature_A  feature_B  feature_C  feature_D
  record 1       1          0          0          0
  record 2       0          0          0          1
  record 3       1          0          0          0          
  record 4       0          1          0          0


  ~~~

  ~~~python
  from sklearn.preprocessing import OneHotEncoder
  ~~~

  * When we fit the encoder, the data need to be **2D array**
  * The result from transforming is sparse matrix, so we need to convert it to dense matrix (well-known matrix)

  ~~~python
  from sklearn.preprocessing import OneHotEncoder
  import numpy as np

  alphabet_features = ["B", "D", "A", "C"]
  alphabet_features = np.array(alphabet_features).reshape(-1, 1)

  label_encoder = OneHotEncoder()
  label_encoder.fit(alphabet_features)

  labels = label_encoder.transform(alphabet_features)

  print(labels)

  print("--------------")

  print(labels.toarray())
  ~~~

  ~~~
  (0, 1)        1.0
  (1, 3)        1.0
  (2, 0)        1.0
  (3, 2)        1.0
  --------------
  [[0. 1. 0. 0.]
  [0. 0. 0. 1.]
  [1. 0. 0. 0.]
  [0. 0. 1. 0.]]

  ~~~

  * We can do one-hot encoding with pandas easily
    * pd.get_dummies(df)

    ~~~python
    import pandas as pd

    alphabet_features = ["B", "D", "A", "C"]
    labels = pd.get_dummies(alphabet_features)

    print(labels)

    ~~~

    ~~~
       A  B  C  D
    0  0  1  0  0
    1  0  0  0  1
    2  1  0  0  0
    3  0  0  1  0
    ~~~


### Feature Scaling
* We set the range of data value toward same levels between all features
* Feature Scaling = Standardization (표준화) + Normalization (정규화)
* Standardization
  * It is usually used when the original data distribution is Gaussian distribution (정규분포)
  * It is efficient when we use SVM, Linear Regression, Logistic Regression because they assume the data distribution is Gaussian distribution
  * **StandardScaler**
    * scaled_value = (value - mean)/std → mean: 0, std: 1
    * Because mean and std are sensitive to the outliers, it is not efficient when there are lots of outliers in data distribution

    ~~~python
    from sklearn.datasets import load_breast_cancer
    from sklearn.preprocessing import StandardScaler
    import pandas as pd

    bc_dataset = load_breast_cancer()

    df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])

    scaler = StandardScaler()
    scaler.fit(df)

    scaled_values = scaler.transform(df)

    scaled_df = pd.DataFrame(data=scaled_values, columns=bc_dataset['feature_names'])

    print(scaled_df)

    print('-------------')

    print(scaled_df.mean().head())

    print('-------------')

    print(scaled_df.var().head())
    ~~~

    ~~~
              mean radius  mean texture  ...  worst symmetry  worst fractal dimension
        0       1.097064     -2.073335  ...        2.750622                 1.937015
        1       1.829821     -0.353632  ...       -0.243890                 0.281190
        2       1.579888      0.456187  ...        1.152255                 0.201391
        3      -0.768909      0.253732  ...        6.046041                 4.935010
        4       1.750297     -1.151816  ...       -0.868353                -0.397100
        ..           ...           ...  ...             ...                      ...
        564     2.110995      0.721473  ...       -1.360158                -0.709091
        565     1.704854      2.085134  ...       -0.531855                -0.973978
        566     0.702284      2.045574  ...       -1.104549                -0.318409
        567     1.838341      2.336457  ...        1.919083                 2.219635
        568    -1.808401      1.221792  ...       -0.048138                -0.751207

        [569 rows x 30 columns]
        -------------
        mean radius       -3.162867e-15
        mean texture      -6.530609e-15
        mean perimeter    -7.078891e-16
        mean area         -8.799835e-16
        mean smoothness    6.132177e-15
        dtype: float64
        -------------
        mean radius        1.001761
        mean texture       1.001761
        mean perimeter     1.001761
        mean area          1.001761
        mean smoothness    1.001761
        dtype: float64


    ~~~

  * **RobustScaler**
    * scaled_value = (value - median)/IQR → median: 0, IQR: 1
    * Because median & IQR are not sensitive to the outliers, we can use it when there are lots of the outliers in the data distribution

    ~~~python
    from sklearn.datasets import load_breast_cancer
    from sklearn.preprocessing import RobustScaler
    import pandas as pd

    bc_dataset = load_breast_cancer()

    df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])

    scaler = RobustScaler()
    scaler.fit(df)

    scaled_values = scaler.transform(df)

    scaled_df = pd.DataFrame(data=scaled_values, columns=bc_dataset['feature_names'])

    print(scaled_df)

    print('-------------')

    print(scaled_df.median().head())

    print('-------------')

    print((scaled_df.quantile(0.75) - scaled_df.quantile(0.25)).head())

    ~~~

    ~~~
          mean radius  mean texture  ...  worst symmetry  worst fractal dimension
     0       1.132353     -1.502664  ...        2.635556                 1.884578
     1       1.764706     -0.190053  ...       -0.106667                 0.435500
     2       1.549020      0.428064  ...        1.171852                 0.365664
     3      -0.477941      0.273535  ...        5.653333                 4.508244
     4       1.696078     -0.799290  ...       -0.678519                -0.158099
     ..           ...           ...  ...             ...                      ...
     564     2.007353      0.630551  ...       -1.128889                -0.431135
     565     1.656863      1.671403  ...       -0.370370                -0.662949
     566     0.791667      1.641208  ...       -0.894815                -0.089234
     567     1.772059      1.863233  ...        1.874074                 2.131911
     568    -1.375000      1.012433  ...        0.072593                -0.467992

     [569 rows x 30 columns]
     -------------
     mean radius        0.0
     mean texture       0.0
     mean perimeter     0.0
     mean area          0.0
     mean smoothness    0.0
     dtype: float64
     -------------
     mean radius        1.0
     mean texture       1.0
     mean perimeter     1.0
     mean area          1.0
     mean smoothness    1.0
     dtype: float64


    ~~~

* Normalization
  * It considers the data value's range more than the data distribution
  * It can be used when the distribution is not Gaussian distribution
    * If we use "Log Transformation", we can let the distribution be similar to Gaussian distribution
  * **MinMaxScaler**
    * scaled_value = (value - min)/(max - min) → data range: [0, 1] (if there is negative data, [-1, 1])
    * Because min & max are sensitive to the outliers, it is not efficient when there are lots of the outliers in the data distribution → RobustScaler can be more efficient than Normalization

    ~~~python
    from sklearn.datasets import load_breast_cancer
    from sklearn.preprocessing import MinMaxScaler
    import pandas as pd

    bc_dataset = load_breast_cancer()

    df = pd.DataFrame(data=bc_dataset['data'],  columns=bc_dataset['feature_names'])

    scaler = MinMaxScaler()
    scaler.fit(df)

    scaled_values = scaler.transform(df)

    scaled_df = pd.DataFrame(data=scaled_values, columns=bc_dataset['feature_names'])

    print(scaled_df)

    print('-------------')

    print(scaled_df.min().head())

    print('-------------')

    print(scaled_df.max().head())


    ~~~

    ~~~
          mean radius  mean texture  ...  worst symmetry  worst fractal dimension
     0       0.521037      0.022658  ...        0.598462                 0.418864
     1       0.643144      0.272574  ...        0.233590                 0.222878
     2       0.601496      0.390260  ...        0.403706                 0.213433
     3       0.210090      0.360839  ...        1.000000                 0.773711
     4       0.629893      0.156578  ...        0.157500                 0.142595
     ..           ...           ...  ...             ...                      ...
     564     0.690000      0.428813  ...        0.097575                 0.105667
     565     0.622320      0.626987  ...        0.198502                 0.074315
     566     0.455251      0.621238  ...        0.128721                 0.151909
     567     0.644564      0.663510  ...        0.497142                 0.452315
     568     0.036869      0.501522  ...        0.257441                 0.100682

     [569 rows x 30 columns]
     -------------
     mean radius        0.0
     mean texture       0.0
     mean perimeter     0.0
     mean area          0.0
     mean smoothness    0.0
     dtype: float64
     -------------
     mean radius        1.0
     mean texture       1.0
     mean perimeter     1.0
     mean area          1.0
     mean smoothness    1.0
     dtype: float64

    ~~~

* **Be careful** when we transform the test_data to scaled_test_data after training the model with scaled_train_data
  * Scaler_object needs to call "fit" only for the train_data
  * For test_data, we need to use (Scaler_object which has already called "fit" with train_data) for transforming it to scaled_test_data
  * **It is more desired to split the train & test data after scaling the whole data (== calling "fit" of scaler_object for the whole data)**
* Tree ML Algorithm doesn't need Scaling
* Which scaler should we use?
  * It is dependent on the situation (We need to test for each scaler)
