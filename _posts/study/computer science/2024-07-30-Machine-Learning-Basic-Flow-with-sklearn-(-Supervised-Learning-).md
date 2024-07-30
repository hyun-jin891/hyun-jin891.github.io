---
layout: post
title: Numpy
description: >
  Python Tool
tags: [data structure]
use_math: true
categories:
  - study
  - computer science
---

### Numpy
* Tools for "ndarray" data
* ndarray: stores identical type of data

## array
* Iterator like list -> ndarray
  * Its dimension can be 1, 2, 3, ...

~~~python
import numpy as np

arr1 = np.array([3, 5, 7])      # 1D
arr2 = np.array([[3, 5, 7]])    # 2D
arr3 = np.array([[3, 5, 7], [5, 8, 9]])          # 3D

~~~

## astype
* The type of ndarray's element can change

~~~python
import numpy as np

arr1 = np.array([3, 5, 7])
arr2 = arr1.astype('float64')
arr3 = arr2.astype('int32')
~~~

## arange
* We can easily make ndarray with integer numbers

~~~python
import numpy as np

arr1 = np.arange(1, 20)  # [1, 2, ..., 19]
~~~

## zeros & ones
* We can easily make ndarray with the shape that we want to make (zeros: all values are 0, ones: all values are 1)
* "dtype=": Default-'float64'

~~~python
import numpy as np

arr1 = np.zeros((3, 4))       # 3x4 Array with 0
arr2 = np.ones((3, 4))        # 3x4 Array with 1

~~~

## reshape
* We can change the shape of ndarray
* The shape should be compatible with the number of elements in ndarray
  * (n, m): n x m = Number of elements in ndarray
* reshape(n, m)
  * 3D: reshape(n, m, k)
  * change the shape to (n, m)
  * reshape(-1, m)
    * It automatically finds the n-value based on the number of elements in ndarray -> **2D Array** regardless of original's dimension
  * reshape(n, -1)
    * It automatically finds the m-value based on the number of elements in ndarray -> **2D Array** regardless of original's dimension
  * **reshape(-1, 1)** (used frequently)
    * **2D Array** regardless of original's dimension with only one column

~~~python
import numpy as np

array = np.arange(1, 17)

arr2 = array.reshape(2, 2, 2, 2)
arr3 = arr2.reshape(-1, 2)

print(array)
print(arr2)
print(arr3)

~~~

~~~
[ 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16]
[[[[ 1  2]
   [ 3  4]]

  [[ 5  6]
   [ 7  8]]]


 [[[ 9 10]
   [11 12]]

  [[13 14]
   [15 16]]]]
[[ 1  2]
 [ 3  4]
 [ 5  6]
 [ 7  8]
 [ 9 10]
 [11 12]
 [13 14]
 [15 16]]
~~~

## Indexing
* One element

~~~python
arr1 = np.array([3, 5, 7])     
arr2 = np.array([[3, 5, 7], [5, 8, 9]])

print(arr1[1])    # 5
print(arr2[1, 2]) # 9
~~~

* Slicing

~~~python
arr1 = np.array([3, 5, 7])     
arr2 = np.array([[3, 5, 7], [1, 8, 9], [10, 4, 6]])

print(arr1[:2])    # [3, 5]  1D
print(arr2[1:, 1:]) # [[8, 9], [4, 6]]  2D
print(arr2[1:, 0])  # [1, 10]  1D
~~~

* Fancy Indexing
  * When its dimension is 2D: If one is fancy indexing, the other one should not be fancy indexing
  * Other ndarray having the index as its data can be the part of fancy indexing (not only list)

~~~python
arr1 = np.array([3, 5, 7])     
arr2 = np.array([[3, 5, 7], [1, 8, 9], [10, 4, 6]])

print(arr1[[1, 2]])      # [5, 7]
print(arr2[[1, 2], 1])   # [8, 4]
print(arr2[[1, 2], 1:])  # [[8, 9], [4, 6]]
print(arr2[[1, 2]])      # [[1, 8, 9], [10, 4, 6]]
~~~

* Boolean Indexing
  * Boolean Expression involving ndarray == ndarray consisting of True/False with same index of target ndarray

~~~python
arr1 = np.array([3, 5, 7])
index = arr1 > 3   # [False, True, True]

arr2 = arr1[index]  # [5, 7]
~~~

## sort
* np.sort(): no side effect (just return)
* ndarray.sort(): side effect (return None)
* np.sort()[::-1]: descending order
* For 2D array
  * axis = 0: sort between the elements that are same column
  * axis = 1: sort between the elements that are same row

~~~python
arr1 = np.array([5, 3, 7])     
arr2 = np.array([[3, 11, 7], [1, 8, 5], [10, 9, 6]])

arr3 = np.sort(arr1)[::-1]   # [7, 5, 3]
arr1.sort()      # [3, 5, 7]

arr4 = np.sort(arr2, axis=0)   # [[1, 8, 5], [3, 9, 6], [10, 11, 7]]
arr5 = np.sort(arr2, axis=1)   # [[3, 7, 11], [1, 5, 8], [6, 9, 10]]

~~~

## argsort
* It returns ndarray of index having original ndarray's index as the elements and this ndarray's index represents the location of these elements when it is sorted

~~~python
arr1 = np.array([5, 3, 7, 6])
arr2 = np.array(["cho", "samoh", "sanford", "rivera"])
sort_index = np.argsort(arr1)    # [1, 0, 3, 2] ndarray

arr3 = arr2[sort_index]   # arr2 can be sorted as well by fancy indexing
                          # ["samoh", "cho", "rivera", "sanford"]
~~~

## dot
* We can implement the multiplication between 2 matrices

~~~python
arr1 = np.array([[1, 4, 3], [5, 7, 6]])
arr2 = np.array([[1, 2], [3, 1], [6, 1]])

arr3 = np.dot(arr1, arr2)   # [[31, 9], [62, 23]]

~~~

## transpose
* We can make transpose matrix

~~~python
arr1 = np.array([[1, 4, 3], [5, 7, 6]])
arr2 = np.transpose(arr1)   # [[1, 5], [4, 7], [3, 6]]
~~~
