---
layout: post
title: Segment Tree
description: >
  Data Structure
tags: [segment_tree]
use_math: true
categories:
  - study
  - computer science
---
### Segment Tree
* When it comes to an array, segment tree is used for solving sum, multiply, minimum, and maximum of the sliced array
* O(logN)
* The size of tree
  * If the size of the array is 10, 16 is the closest to 10 in N<sup>2</sup> (> 10)
  * 16 "x 2" = 32
  * Consequently, 32 is the number of data needed for making the tree
  * Generally, we just set the size by N "x 4"
* Index of segment tree starts from 1
* Left node's value : start (root) ~ mid (root)
* Right node's value : mid + 1 ~ end (root)
* Example : a = [1, 2, 3, 4, 5, 6, 7]
  * Sum
  ![그림1](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/142.PNG?raw=true){: width="600" height="600"}<br>

  * Minimum
  ![그림2](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/143.PNG?raw=true){: width="600" height="600"}<br>

    * Node's value is an index of the minimum value

### Initialization
* sum<br>

~~~python
tree = [0] * (N * 4)

def init(start, end, index):
  if start == end:
    tree[index] = start
    return tree[index]

  mid = (start + end) // 2
  tree[index] = init(start, mid, index * 2) + init(mid + 1, end, index * 2 + 1)

  return tree[index]
~~~

Use recursion<br>
index * 2 means the left node<br>
index * 2 + 1 means the right node<br>

-------------------

* Minimum<br>

~~~python
tree = [0] * (N * 4)

def init(start, end, index):
  if start == end:
    tree[index] = start
    return tree[index]

  mid = (start + end) // 2

  init(start, mid, index * 2)
  init(mid + 1, end, index * 2 + 1)

  if array[tree[index * 2]] < array[tree[index * 2 + 1]]:
    tree[index] = tree[index * 2]
  else:
    tree[index] = tree[index * 2 + 1]

  return tree[index]
~~~

Tree's value == **index** for minimum value<br>


### Query Function
* specific section of array [left, right] → solve the sum, ....
* If [start, end] (section of a specific node) is involved in [left, right], tree[index]'s value is directly used
* If start > right or end < left, ignore
* else: check the left node and right node
* Sum<br>

~~~python
def query(start, end, index, left, right):
  if start > right or end < left:
    return 0                       ## 0 is "ignore" in sum process

  if start >= left and end <= right:
    return tree[index]

  mid = (start + end) // 2

  return query(start, mid, index * 2, left, right) + query(mid + 1, end, index * 2 + 1, left, right)
~~~

<br>

* Minimum<br>

~~~python
def query(start, end, index, left, right):
  if start > right or end < left:
    return -1                    # -1 gonna be flag for knowing whether it is "ignore" or not

  if start >= left and end <= right:
    return tree[index]

  mid = (start + end) // 2
  index_left = query(start, mid, index * 2, left, right)
  index_right = query(mid + 1, end, index * 2 + 1, left, right)

  if index_left == -1 and index_right == -1:
    return -1

  if index_left == -1:
    return index_right

  if index_right == -1:
    return index_left

  if array[index_left] < array[index_right]:
    return index_left
  else:
    return index_right
~~~

<br>

### Update Function
* If an element in array is modified, segment tree should also be modified
* It is okay to check the node corresponding to the sections which involve the element
* Sum<br>

~~~python
def update(start, end, index, what, value):
  if what < start or what > end:
    return

  tree[index] += value  # difference between original result and new result
  if start == end:
    return

  mid = (start + end) // 2
  update(start, mid, index * 2, what, value)
  update(mid + 1, end, index * 2 + 1, what, value)

~~~

<br>

* Minimum<br>

~~~python
def update(start, end, index, what):
  if what < start or what > end:
    return -1

  if start == end:
    tree[index] = start
    return tree[index]

  mid = (start + end) // 2
  index_left = update(start, mid, index * 2, what)
  index_right = update(mid + 1, end, index * 2 + 1, what)

  if index_left == -1 and index_right == -1:
    return -1
  if index_left == -1:
    return index_right
  if index_right == -1:
    return index_left

  if array[index_left] < array[index_right]:
    return index_left
  else:
    return index_right

~~~
