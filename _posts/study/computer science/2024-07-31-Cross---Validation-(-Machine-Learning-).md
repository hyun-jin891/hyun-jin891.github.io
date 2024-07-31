---
layout: post
title: Cross-Validation (Machine Learning)
description: >
  Machine Learning
tags: [machine learning]
use_math: true
categories:
  - study
  - computer science
---
### Cross-Validation (Machine Learning)
* It is needed for solving "Overfitting"

### Overfitting
* 과적합
* model's excessive optimization of a only given train data
* Cross-Validation gives an opportunity for the model learning with multiple train datasets
  * K Fold Cross-Validation
  * Stratified K Fold Cross-Validation

### K Fold
* (Split the records into K fold) x K
  * Fold = train_fold x (K - 1) + test_fold x 1

~~~
EX) K = 4
                                DATA
1 Try      Train_Fold 1 | Train_Fold 2 | Train_Fold3 | Test_Fold
2 Try      Train_Fold 1 | Train_Fold 2 | Test_Fold | Train_Fold3
3 Try      Train_Fold 1 | Test_Fold | Train_Fold 2 | Train_Fold3
4 Try      Test_Fold | Train_Fold 1 | Train_Fold 2 | Train_Fold3

Final Evaluation: mean(Evaluation 1, Evaluation 2, Evaluation 3, Evaluation 4)

~~~

~~~python
from sklearn.model_selection import KFold

kfold_obj.split(data) # iterator for (train_indexes, test_indexes) -> They are used for fancy indexing
~~~

<br>

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score
from sklearn.model_selection import KFold
import numpy as np
import pandas as pd

bc_dataset = load_breast_cancer()

df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
df['clf_label'] = bc_dataset['target']

df_x = df.iloc[:, :-1]
df_y = df['clf_label']
k_fold = KFold(n_splits=4)

score_list = []

for train_index, test_index in k_fold.split(df_x): # no need for df_y (∵ Its index is same with features dataset)
   X_train, Y_train = df_x.iloc[train_index], df_y.iloc[train_index]
   X_test, Y_test = df_x.iloc[test_index], df_y.iloc[test_index]

   clf_model = DecisionTreeClassifier(random_state=1)
   clf_model.fit(X_train, Y_train)
   res = clf_model.predict(X_test)

   score_list.append(np.round(accuracy_score(Y_test, res), 2))

print(np.mean(score_list))
~~~

~~~
0.9075
~~~

### Stratified K Fold
* In the case of "Classification"
  * If distribution of label dataset is imbalanced, each fold may not reflect the original label dataset's distribution
    * ex) certain fold didn't learn 0 label at all
  * Stratified K Fold makes the folds whose distribution is similar to the original dataset
* In the case of "Regression"
  * We don't need to consider the original dataset's distribution
  * sklearn supports only K Fold (not Stratified K fold)

~~~python
from sklearn.model_selection import StratifiedKFold
~~~

* The only difference between K Fold and Stratified K Fold is kfold_obj.split's parameters
  * K Fold don't need to use label dataset as a parameter because its index is same with the features' dataset
  * In the case of Stratified K Fold, it has to consider the original label dataset's distribution
    * Consequently, it needs not only the features' dataset, but the label dataset

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score
from sklearn.model_selection import StratifiedKFold
import numpy as np
import pandas as pd

bc_dataset = load_breast_cancer()

df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
df['clf_label'] = bc_dataset['target']

df_x = df.iloc[:, :-1]
df_y = df['clf_label']
k_fold = StratifiedKFold(n_splits=4)

score_list = []
clf_model = DecisionTreeClassifier(random_state=1)

for train_index, test_index in k_fold.split(df_x, df_y):
   X_train, Y_train = df_x.iloc[train_index], df_y.iloc[train_index]
   X_test, Y_test = df_x.iloc[test_index], df_y.iloc[test_index]

   clf_model.fit(X_train, Y_train)
   res = clf_model.predict(X_test)

   score_list.append(np.round(accuracy_score(Y_test, res), 2))

print(np.mean(score_list))

~~~

~~~
0.9225
~~~

### cross_val_score()
* make cross-validation easier
* Major Parameters
  * estimator
    * If estimator is for "Classification", it will split the dataset through Stratified K Fold
    * If estimator is for "Regression", it will split the dataset through K Fold
  * x: independent variable
  * y: dependent variable
  * scoring: put a kind of evaluation (If we use "cross_validate()", it will return several kinds of evaluation)
  * cv: number of folds
* Return
  * Array with scores

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import cross_val_score
import numpy as np
import pandas as pd

bc_dataset = load_breast_cancer()

df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
df['clf_label'] = bc_dataset['target']

df_x = df.iloc[:, :-1]
df_y = df['clf_label']

clf_model = DecisionTreeClassifier(random_state=1)

scores = cross_val_score(clf_model, df_x, df_y, scoring='accuracy', cv=4)


print(np.mean(scores))
~~~

~~~
0.9244927607603665
~~~

### GridSearchCV
* It gives an opportunity for doing both (cross-validation) and (hyperparameter tunning for optimization) simultaneously
* Hyperparameter
  * Each machine learning algorithm has their several hyperparameters that can be tunned for optimization
  * Example for "Classification"
    * max_depth (hyperparameter) = [4, 2, 5]
    * min_samples_split (hyperparameter) = [3, 5]
    * Possible case for hyperparameter's combination = 3 x 2 = 6
    * We should find the best combination for optimization
* It takes a while relatively for finding the best combination of hyperparameter
  * If cv = 4 & number of hyperparameter's combinations = 6, its number of calculation is 6 x 4 = 24
* Procedure
  * 1<sup>st</sup>: Split X_train, X_test, Y_train, Y_test
  * 2<sup>nd</sup>: Do cross-validation with X_train, Y_train (GridSearchCV)
  * 3<sup>rd</sup>: Finding the best combination of hyperparameter → train the model again with these hyperparameters again (GridSearchCV)
  * 4<sup>th</sup>: Evaluation of these model with X_test, Y_test
* Parameters of GridSearchCV's constructor
  * estimator
  * param_grid: Dictionary (key: hyperparameter, value: list of candidate value)
  * scoring
  * cv
  * refit: If it is True, GridSearchCV let the estimator learn again with tunned hyperparameter (Default: True)

~~~python
from sklearn.model_selection import GridSearchCV
~~~

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score
from sklearn.model_selection import GridSearchCV
import numpy as np
import pandas as pd

bc_dataset = load_breast_cancer()

df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
df['clf_label'] = bc_dataset['target']

df_x = df.iloc[:, :-1]
df_y = df['clf_label']

X_train, X_test, Y_train, Y_test = train_test_split(df_x, df_y, test_size=0.2, random_state=1, stratify=df_y)

clf_model = DecisionTreeClassifier(random_state=1)

param_dict = {'max_depth' : [4, 2, 5], 'min_samples_split' : [3, 5]}

grid_search_cv = GridSearchCV(clf_model, param_grid=param_dict, scoring='accuracy', cv=4)

grid_search_cv.fit(X_train, Y_train)

res_df = pd.DataFrame(grid_search_cv.cv_results_) # cv_results_: result per each case (cv x comb of hyperparam)
print(res_df)
print("-----------------")

best_param = grid_search_cv.best_params_ # best comb of hyperparam
print(best_param)
print("-----------------")

best_score = grid_search_cv.best_score_ # best score
print(best_score)
print("-----------------")

new_model = grid_search_cv.best_estimator_ # Tunned model

res = new_model.predict(X_test)

print(np.round(accuracy_score(Y_test, res), 2))
~~~

~~~
mean_fit_time  std_fit_time  mean_score_time  std_score_time  ... split3_test_score mean_test_score std_test_score  rank_test_score
0       0.005249      0.000433         0.002001        0.000707  ...          0.929204        0.929669       0.038737                4
1       0.004749      0.000433         0.001752        0.000434  ...          0.920354        0.929650       0.035676                5
2       0.003749      0.000432         0.001501        0.000500  ...          0.920354        0.931843       0.036945                2
3       0.003499      0.000500         0.001501        0.000501  ...          0.920354        0.931843       0.036945                2
4       0.005494      0.000869         0.001752        0.000433  ...          0.920354        0.929650       0.031677                5
5       0.005248      0.000829         0.001502        0.000500  ...          0.938053        0.934075       0.031304                1

[6 rows x 14 columns]
-----------------
{'max_depth': 5, 'min_samples_split': 5}
-----------------
0.9340746778450552
-----------------
0.96
~~~
