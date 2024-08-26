---
layout: post
title: Classification; Decision Tree (Machine Learning)
description: >
  Machine Learning
tags: [machine learning]
use_math: true
categories:
  - study
  - computer science
---
### Classification; Decision Tree (Machine Learning)
* Decision Tree is ML algorithm which can be used for regression and classification
* Each node in the tree has a rule when it comes to the specific feature → split the sample based on the rule (True or False)

~~~python
from sklearn.tree import DecisionTreeClassifier
~~~

* Basic Structure

~~~
          Rule node1
          ↙       ↘   Split based on the rule
    Rule node2      Rule node3
                .
                .
                .
    leaf node: Determined Label
~~~


### Uniformity
* 균일도
* When the identical data are many in the data sample, the uniformity is high
* When we make the decision tree, it should split the data sample with the direction of increasing uniformity
* Representation of Uniformity
  * Information Gain (정보 이득)
    * Entrophy ↑ → Uniformity ↓
    * Information Gain = 1 - entrophy
  * Gini-Index (지니 계수)
    * Gini-Index ↓ → Uniformity ↑
    * DecisionTreeClassifier uses gini-index for checking the uniformity of the sample

### Property of Decision Tree
* Decision Tree considers only uniformity, so it is easy to understand intuitively
* It is less important to do preprocessing like scaling because it considers only uniformity
* **Overfitting**
  * Decision Tree has chance of going through overfitting
  * If we don't make the limitation of tree's depth, it may continue making the rule nodes until it perfectly does classification for the given data (even outliers gonna be considered to the model) → Decision Tree will be complex → It will be efficient to only train data (overfitting)
  * For avoiding the overfitting, we should find the best combinations of the parameters

### Decision Tree Parameters
* DecisionTreeClassifier & DecisionTreeRegressor have the same parameters
* Kinds of Parameters
  * "min_samples_split"
    * If size(current sample) < min_samples_split, it will not split
    * min_samples_split ↓ → split ↑ → Overfitting ↑
  * "min_samples_leaf"
    * If size(next_left_node) or size(next_right_node) < min_samples_leaf, it will not split
    * min_samples_leaf ↓ → split ↑ → Overfitting ↑
  * "max_features"
    * For the best split, the number of features that will be considered
    * Default: None → All features are used
    * int: the number of features
    * float: the percent of features
    * sqrt: sqrt(the number of all features)
    * ....
  * "max_depth"
    * the maximum depth of the tree
    * It should be modulated for avoiding the overfitting
  * "max_leaf_nodes"
    * the maximum number of the leaf nodes

### Feature Importances
* Decision Tree can give the importance of each feature that are used for spliting the nodes

~~~python
clf_dt = DecisionTreeClassifier()

#fit, predict

print(clf_dt.feature_importances_)
~~~

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
import pandas as pd
import numpy as np

bc_dataset = load_breast_cancer()

df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
df['clf_label'] = bc_dataset['target']

df_x = df.iloc[:, :-1]
df_y = df['clf_label']


X_train, X_test, Y_train, Y_test = train_test_split(df_x, df_y, test_size=0.2, random_state=1, stratify=df_y)

clf_model = DecisionTreeClassifier()
clf_model.fit(X_train, Y_train)
pred = clf_model.predict(X_test)

print(clf_model.feature_importances_)
~~~

~~~
[0.         0.02477658 0.         0.         0.         0.
 0.         0.         0.         0.0125215  0.01210157 0.00626075
 0.         0.         0.         0.         0.         0.00785663
 0.00866873 0.         0.69006619 0.0496551  0.00883871 0.00194409
 0.         0.         0.03382403 0.14348612 0.         0.        ]
~~~
