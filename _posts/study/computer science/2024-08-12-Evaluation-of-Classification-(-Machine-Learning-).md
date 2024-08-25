---
layout: post
title: Evaluation of Classification (Machine Learning)
description: >
  Machine Learning
tags: [machine learning]
use_math: true
categories:
  - study
  - computer science
---
### Evaluation of Classification (Machine Learning)
* There are evaluation metrics of "Classification"
  * Confusion Matrix
  * Accuracy
  * Precision
  * F1 Score
  * ROC AUC
* They are efficient to both binary classification and multiclass classification
  * especially binary classification
* We can use sklearn
  * **evaluation_method(Y_test, prediction)**

### Confusion Matrix
* If labels are A, B, C (3 Classification), confusion matrix is 3 x 3

  ~~~
        A-A    A-B    A-C
        B-A    B-B    B-C
        C-A    C-B    C-C

        A-B: Prediction = B, Actual Label = A
  ~~~

* If there are N labels, confusion matrix is N x N
* If labels are N (negative), P (positive) : that is, it is binary classification
  * Confusion matrix consists of TN, FP, FN, TP (2 x 2)
  * TN (True Negative): Prediction = N, Actual label = N
  * FP (False Positive): Prediction = P, Actual label = N
  * FN (False Negative): Prediction = N, Actual label = P
  * TP (True Positive): Prediction = P, Actual label = P

  ~~~
        TN | FP
        ㅡㅡㅡㅡ
        FN | TP
  ~~~

* We can use sklearn for "Confusion Matrix"

~~~python
from sklearn.metrics import confusion_matrix
~~~

* Binary Classification

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import confusion_matrix
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

print(confusion_matrix(Y_test, res))
~~~

~~~
[[39  3]
 [ 1 71]]
~~~

### Accuracy
* **Accuracy = (Number of prediction = actual_label)/(total data)**
* In binary classification
  * Accuracy = (TN + TP)/(TN + FP + FN + TP)
  * **It is dangerous to use only "Accuracy" for evaluation**
    * If data distribution is like N : 99, P: 1 in the specific condition, the model simply selecting N in this condition can get the high score in accuracy
    * We can understand this fact through "BaseEstimator" supporting for us to design our estimator

    ~~~python
    from sklearn.base import BaseEstimator
    from sklearn.model_selection import train_test_split
    import numpy as np
    import pandas as pd
    from sklearn.metrics import accuracy_score

    class MyEstimator(BaseEstimator):
      def fit(self, X, Y):
          pass

      def predict(self, X):
          res = np.zeros((X.shape[0], 1))

          for i in range(X.shape[0]):
              if X["class"].iloc[i] == 'A':
                  res[i] = 1
              else:
                  res[i] = 0

              return res

    df = pd.DataFrame({"number" : [1, 2, 3, 4, 5], "class" : ['A', 'C', 'A', 'A', 'W'], "radius" : [2.1, 3.5, 4.5, 13.5, 20], "labels" : [1, 0, 1, 1, 0]})          
    x = df.iloc[:, :-1]
    y = df['labels']


    X_train, X_test, Y_train, Y_test = train_test_split(x, y, test_size=0.2)
    model = MyEstimator()
    model.fit(X_train, Y_train)
    res = model.predict(X_test)

    print(np.round(accuracy_score(Y_test, res), 2))  

    ~~~

    * MyEstimator: simply select 1 or 0 based on the "class" value
    * Its accuracy == 1.0
* We can use sklearn for "Accuracy"

~~~python
from sklearn.metrics import accuracy_score
~~~

* Binary Classification

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score
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

print(accuracy_score(Y_test, res))

~~~

~~~
0.9649122807017544
~~~

### Precision & Recall
* Precision = TP / (FP + TP)
  * TP ratio of the data being predicted as Positive
  * Positive Predictive Value (양성예측도)
* Recall = TP / (FN + TP)
  * TP ratio of the data which are actually Positive
  * Sensitivity (민감도) or TPR (True Positive Rate)
* Precision vs Recall
  * Precision is more important when the case of negative data being predicted as positive is more fatal (Situation is sensitive to FP)
  * Recall is more important when the case of positive data being predicted as negative is more fatal (Situation is sensitive to FN)
* Precision & Recall with sklearn

~~~python
from sklearn.metrics import precision_score, recall_score
~~~

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import precision_score, recall_score
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

print(precision_score(Y_test, res))
print(recall_score(Y_test, res))
~~~

~~~
0.9594594594594594
0.9861111111111112
~~~

* Precision & Recall Trade-off
  * Tendency: precision ↑ → recall ↓ or recall ↑ → precision ↓
  * We can modulate precision or recall through changing **threshold probability**

### Threshold in Binary Classification
* We can easily understand this concept by using **"Binarizer" class**: This class is used to do binary classification with various thresholds (is used to let other binary classification algorithm be done with various thresholds)

    ~~~python
    from sklearn.preprocessing import Binarizer

    # binarizer = Binarizer(threshold=n).fit(X)
    # pred = binarizer.transform(X)
    # X: 2D array, element > threshold → 1
    # It is better for X to involve only one column (X[:, 1].reshape(-1, 1)) because it can make decision with only one column (0 label or 1 label)
    ~~~

    * Here, X is the result from "predict_proba(X_test)"
      * model.predict_proba(X_test): collect the decision probability (calculated probability from the model)

    ~~~python
    from sklearn.datasets import load_breast_cancer
    from sklearn.model_selection import train_test_split
    from sklearn.linear_model import LogisticRegression
    import pandas as pd

    bc_dataset = load_breast_cancer()

    df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
    df['clf_label'] = bc_dataset['target']

    df_x = df.iloc[:, :-1]
    df_y = df['clf_label']


    X_train, X_test, Y_train, Y_test = train_test_split(df_x, df_y, test_size=0.2, random_state=1, stratify=df_y)

    clf_model = LogisticRegression()
    clf_model.fit(X_train, Y_train)
    pred_prob = clf_model.predict_proba(X_test)

    print(pred_prob)
    ~~~

    ~~~
    [[6.82292187e-02 9.31770781e-01]   # 0's probability = 0.068.. vs 1's probability = 0.93...
     [7.70342883e-01 2.29657117e-01]
     [9.39147679e-02 9.06085232e-01]
     [5.21386736e-03 9.94786133e-01]
     [9.99999921e-01 7.91484082e-08]
     [2.44665087e-02 9.75533491e-01]
     [5.10401968e-02 9.48959803e-01]
     [5.16881669e-02 9.48311833e-01]
     [1.00000000e+00 4.91786109e-12]
     [7.18840618e-03 9.92811594e-01]
     [2.07073120e-02 9.79292688e-01]
     [1.65964862e-02 9.83403514e-01]
     [2.52298240e-04 9.99747702e-01]
     [8.75514987e-01 1.24485013e-01]
     [6.48546863e-02 9.35145314e-01]
     [3.10829769e-03 9.96891702e-01]
     [4.20432835e-02 9.57956716e-01]
     [9.99979635e-01 2.03652096e-05]
     [1.30302187e-03 9.98696978e-01]
     [9.59692133e-01 4.03078672e-02]
     [2.47784418e-02 9.75221558e-01]
     [4.19578143e-01 5.80421857e-01]
     [3.64262519e-03 9.96357375e-01]
     [1.95188532e-02 9.80481147e-01]
     [2.73468194e-02 9.72653181e-01]
     [1.00000000e+00 5.46731071e-16]
     [9.98906523e-01 1.09347653e-03]
     [1.79732455e-03 9.98202675e-01]
     [9.99999951e-01 4.90565569e-08]
     [1.00000000e+00 2.71248587e-12]
     [9.99999972e-01 2.81592934e-08]
     [9.99994820e-01 5.17956331e-06]
     [9.99379890e-01 6.20110088e-04]
     [9.99999761e-01 2.39133181e-07]
     [2.36623030e-01 7.63376970e-01]
     [9.99999999e-01 7.89803794e-10]
     [8.98467898e-02 9.10153210e-01]
     [1.00000000e+00 1.07245529e-18]
     [1.00000000e+00 5.49285121e-15]
     [3.23877236e-03 9.96761228e-01]
     [8.00380958e-03 9.91996190e-01]
     [1.21777781e-03 9.98782222e-01]
     [9.99680919e-01 3.19081490e-04]
     [9.99999931e-01 6.90839506e-08]
     [9.99999706e-01 2.93974716e-07]
     [9.99996432e-01 3.56823629e-06]
     [6.04790899e-01 3.95209101e-01]
     [5.50599090e-03 9.94494009e-01]
     [4.51930430e-04 9.99548070e-01]
     [8.99483806e-02 9.10051619e-01]
     [1.12558603e-03 9.98874414e-01]
     [1.30788879e-04 9.99869211e-01]
     [8.26146308e-03 9.91738537e-01]
     [6.05050735e-01 3.94949265e-01]
     [9.99999988e-01 1.21218105e-08]
     [9.99999881e-01 1.18797925e-07]
     [6.56929192e-03 9.93430708e-01]
     [7.80043017e-03 9.92199570e-01]
     [9.24449739e-04 9.99075550e-01]
     [3.67785037e-03 9.96322150e-01]
     [2.12625187e-04 9.99787375e-01]
     [1.10696487e-01 8.89303513e-01]
     [1.94127003e-03 9.98058730e-01]
     [1.73244741e-03 9.98267553e-01]
     [6.77557362e-01 3.22442638e-01]
     [3.57080723e-03 9.96429193e-01]
     [2.22604816e-04 9.99777395e-01]
     [1.22079663e-02 9.87792034e-01]
     [9.96180827e-01 3.81917298e-03]
     [2.18560446e-02 9.78143955e-01]
     [9.96594697e-01 3.40530325e-03]
     [1.51520165e-04 9.99848480e-01]
     [2.18124600e-01 7.81875400e-01]
     [8.68155618e-01 1.31844382e-01]
     [3.97918633e-02 9.60208137e-01]
     [3.21949696e-03 9.96780503e-01]
     [1.00000000e+00 5.80135872e-15]
     [2.31178528e-02 9.76882147e-01]
     [3.69630360e-03 9.96303696e-01]
     [5.83547200e-03 9.94164528e-01]
     [9.99187721e-01 8.12279135e-04]
     [3.18204027e-02 9.68179597e-01]
     [1.84675756e-01 8.15324244e-01]
     [9.99999999e-01 1.25013560e-09]
     [1.89450555e-02 9.81054944e-01]
     [1.39243723e-03 9.98607563e-01]
     [2.51615318e-04 9.99748385e-01]
     [9.99999930e-01 7.00031187e-08]
     [1.97969472e-02 9.80203053e-01]
     [3.81565851e-03 9.96184341e-01]
     [7.34902793e-03 9.92650972e-01]
     [1.00000000e+00 8.34192672e-15]
     [5.48233128e-03 9.94517669e-01]
     [5.28945301e-02 9.47105470e-01]
     [3.32803634e-02 9.66719637e-01]
     [4.35884325e-03 9.95641157e-01]
     [1.00000000e+00 3.09656525e-11]
     [1.93952794e-03 9.98060472e-01]
     [9.99999345e-01 6.54707796e-07]
     [1.20173910e-01 8.79826090e-01]
     [7.06824475e-03 9.92931755e-01]
     [9.99999397e-01 6.03330696e-07]
     [1.62717947e-03 9.98372821e-01]
     [4.61398485e-02 9.53860151e-01]
     [1.00000000e+00 1.76305233e-12]
     [6.37290630e-02 9.36270937e-01]
     [1.59041333e-01 8.40958667e-01]
     [3.88427968e-01 6.11572032e-01]
     [1.04047497e-03 9.98959525e-01]
     [5.24622316e-03 9.94753777e-01]
     [9.99999915e-01 8.46681995e-08]
     [9.99909959e-01 9.00408217e-05]
     [9.98333510e-01 1.66648992e-03]
     [4.02778314e-04 9.99597222e-01]]
    ~~~

    ~~~python
    from sklearn.datasets import load_breast_cancer
    from sklearn.model_selection import train_test_split
    from sklearn.preprocessing import Binarizer
    from sklearn.tree import DecisionTreeClassifier
    from sklearn.linear_model import LogisticRegression
    from sklearn.metrics import precision_score, recall_score
    import pandas as pd

    bc_dataset = load_breast_cancer()

    df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
    df['clf_label'] = bc_dataset['target']

    df_x = df.iloc[:, :-1]
    df_y = df['clf_label']


    X_train, X_test, Y_train, Y_test = train_test_split(df_x, df_y, test_size=0.2, random_state=1, stratify=df_y)

    clf_model = LogisticRegression(solver='liblinear')
    clf_model.fit(X_train, Y_train)
    pred_prob = clf_model.predict_proba(X_test)[:, 1].reshape(-1, 1)

    binarizer = Binarizer(threshold=0.5).fit(pred_prob)
    pred = binarizer.transform(pred_prob)

    print("Threshold 0.5")
    print()
    print(precision_score(Y_test, pred))
    print(recall_score(Y_test, pred))
    print()

    print("Threshold 0.3")
    print()

    binarizer = Binarizer(threshold=0.3).fit(pred_prob)
    pred = binarizer.transform(pred_prob)

    print(precision_score(Y_test, pred))
    print(recall_score(Y_test, pred))
    print()

    print("Threshold 0.7")
    print()

    binarizer = Binarizer(threshold=0.7).fit(pred_prob)
    pred = binarizer.transform(pred_prob)

    print(precision_score(Y_test, pred))
    print(recall_score(Y_test, pred))
    ~~~

    ~~~
    Threshold 0.5

    0.9726027397260274
    0.9861111111111112

    Threshold 0.3

    0.9342105263157895
    0.9861111111111112

    Threshold 0.7

    0.9726027397260274
    0.9861111111111112
    ~~~

    ~~~python
    # Function getting various thresholds and doing evaluation for binary classification
    from sklearn.datasets import load_breast_cancer
    from sklearn.model_selection import train_test_split
    from sklearn.preprocessing import Binarizer
    from sklearn.tree import DecisionTreeClassifier
    from sklearn.linear_model import LogisticRegression
    from sklearn.metrics import precision_score, recall_score
    import pandas as pd

    bc_dataset = load_breast_cancer()

    df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
    df['clf_label'] = bc_dataset['target']

    df_x = df.iloc[:, :-1]
    df_y = df['clf_label']


    X_train, X_test, Y_train, Y_test = train_test_split(df_x, df_y, test_size=0.2, random_state=1, stratify=df_y)

    clf_model = LogisticRegression(solver='liblinear')
    clf_model.fit(X_train, Y_train)
    pred_prob = clf_model.predict_proba(X_test)[:, 1].reshape(-1, 1)


    thresholds = [0.3, 0.45, 0.6, 0.75, 0.9]
    def binClf_thresholds(Y_test, pred_prob, thresholds):
      for threshold in thresholds:
          binarizer = Binarizer(threshold=threshold).fit(pred_prob)
          res = binarizer.transform(pred_prob)

          print("Threshold:", threshold)
          print(precision_score(Y_test, res))
          print(recall_score(Y_test, res))
          print()

    binClf_thresholds(Y_test, pred_prob, thresholds)
    ~~~

    ~~~
    Threshold: 0.3
    0.9342105263157895
    0.9861111111111112

    Threshold: 0.45
    0.9594594594594594
    0.9861111111111112

    Threshold: 0.6
    0.9726027397260274
    0.9861111111111112

    Threshold: 0.75
    0.971830985915493
    0.9583333333333334

    Threshold: 0.9
    0.9848484848484849
    0.9027777777777778
    ~~~

    * "precision_recall_curve"

    ~~~python
    from sklearn.metrics import precision_recall_curve

    precisions, recalls, thresholds = precision_recall_curve(Y_test, pred_prob)

    # It gives precisions and recalls per each threshold

    ~~~

### F1 Score
* combine precision with recall
  * F1 score = 2 x (precision * recall)/(precision + recall)

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import Binarizer
from sklearn.tree import DecisionTreeClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import f1_score
import pandas as pd
import numpy as np

bc_dataset = load_breast_cancer()

df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
df['clf_label'] = bc_dataset['target']

df_x = df.iloc[:, :-1]
df_y = df['clf_label']


X_train, X_test, Y_train, Y_test = train_test_split(df_x, df_y, test_size=0.2, random_state=1, stratify=df_y)

clf_model = LogisticRegression(solver='liblinear')
clf_model.fit(X_train, Y_train)
pred = clf_model.predict(X_test)

print(np.round(f1_score(Y_test, pred), 2))
~~~

~~~
0.98
~~~

### ROC Curve & AUC
* ROC Curve
  * Curve with 1-specificity (x-axis) and sensitivity (y-axis)
    * Specificity = TN/(TN + FP)
      * True rate in actual negative group
    * Sensitivity = TP/(TP + FN) = Recall
      * True rate in actual positive group
  * Our goal is to increase both specificity and sensitivity
    * The curve of good model should be far away from y = x (when we increase specificity, the sensitivity should be maintained at the high level)
  * "roc_curve()"

  ~~~python
  from sklearn.metrics import roc_curve()

  x_axis, y_axis, thresholds = roc_curve(Y_test, pred_prob)

  # It gives (1-specificity, sensitivity) per each threshold
  # pred_prob doesn't need to be 2D

  ~~~

  * We can make ROC curve with matplotlib
* AUC (Area under Curve)
  * AUC of good model should be close to 1 (Area of square)

~~~python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import Binarizer
from sklearn.tree import DecisionTreeClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import roc_auc_score
import pandas as pd
import numpy as np

bc_dataset = load_breast_cancer()

df = pd.DataFrame(data=bc_dataset['data'], columns=bc_dataset['feature_names'])
df['clf_label'] = bc_dataset['target']

df_x = df.iloc[:, :-1]
df_y = df['clf_label']


X_train, X_test, Y_train, Y_test = train_test_split(df_x, df_y, test_size=0.2, random_state=1, stratify=df_y)

clf_model = LogisticRegression(solver='liblinear')
clf_model.fit(X_train, Y_train)
pred_prob = clf_model.predict_proba(X_test)[:, 1].reshape(-1, 1)


print(np.round(roc_auc_score(Y_test, pred_prob), 2))
~~~

~~~
0.99
~~~
