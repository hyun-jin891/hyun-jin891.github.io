---
layout: post
title: Quick Sort
description: >
  Algorithm
tags: [quick sort]
use_math: true
categories:
  - study
  - computer science
---
### Quick Sort
* a kind of sorting algorithm
* use Divide & Conquer

### Property
* fast
* O(nlogn)
* Slow when most of elements have already been sorted (or already been reversely sorted)

### Implementation
* Procedure
  * pick pivot (middle point)
  * collect front_elements (x < pivot)
  * collect rear_elements (x > pivot)
  * collect pivot_elements (x == pivot)
  * Use recursion
    * return ***quickSort(front) + quickSort(pivot) + quickSort(rear)***

~~~Python
def quick_sort(array):
    if len(array) <= 1:
        return array

    pivot = len(array) // 2
    front_arr, pivot_arr, rear_arr = [], [], []

    for value in array:
        if value < array[pivot]:
            front_arr.append(value)
        elif value > array[pivot]:
            rear_arr.append(value)
        else:
            pivot_arr.append(value)
    if len(front_arr) == 0 and len(back_arr) == 0 and len(pivot_arr) != 0:
        return pivot_arr
    return quick_sort(front_arr) + quick_sort(pivot_arr) + quick_sort(back_arr)
~~~

* code Analysis<br>

~~~Python
if len(front_arr) == 0 and len(back_arr) == 0 and len(pivot_arr) != 0:
    return pivot_arr
~~~
This code is for dealing with the issue of elements which are in arrays as multiple times<br>

-----
