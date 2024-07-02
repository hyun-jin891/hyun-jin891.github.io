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
* Procedure 1
  * pick pivot (middle point)
  * collect front_elements (x < pivot)
  * collect rear_elements (x > pivot)
  * collect pivot_elements (x == pivot)
  * Use recursion
    * return ***quickSort(front) + quickSort(pivot) + quickSort(rear)*** (Extending of front_list, pivot, rear_list)

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

<br>

~~~java
import java.util.ArrayList;
import java.util.Arrays;

public class QuickSortImplementation2 {

	public static void main(String[] args) {
		Integer[] array = {2, 4, 3, 6, 5, 8, 10, 1, 7, 9};

		ArrayList<Integer> sortedList = quickSort(new ArrayList<Integer>(Arrays.asList(array)));

		System.out.println(sortedList);

	}

	static <T extends Comparable<T>> ArrayList<T> quickSort(ArrayList<T> array) {
		int start = 0;
		int end = array.size() - 1;

		if (array.size() == 0) {
			ArrayList<T> result = new ArrayList<T>();
			return result;
		}

		int mid = (start + end) / 2;

		ArrayList<T> preList = new ArrayList<T>();
		ArrayList<T> postList = new ArrayList<T>();

		for (int i = 0; i < array.size(); i++) {
			if (i == mid)
				continue;
			if (array.get(i).compareTo(array.get(mid)) <= 0)
				preList.add(array.get(i));
			else
				postList.add(array.get(i));
		}

		ArrayList<T> preResult = quickSort(preList);
		ArrayList<T> postResult = quickSort(postList);

		preResult.addAll(new ArrayList<T>(Arrays.asList(array.get(mid))));
		preResult.addAll(postResult);

		return preResult;

	}


}
~~~

* Code Analysis
  * It is similar with Python Code
  * For controlling easily like extending of arrays, I use ArrayList Class instead of array

-----

<br>

~~~c
int* quickSort(int* array, int size)
{
    int start = 0;
    int end = size - 1;
    int mid = (start + end) / 2;

    int* preList = (int*)malloc(sizeof(int) * (size));
    int preIndex = 0;
    int* postList = (int*)malloc(sizeof(int) * (size));
    int postIndex = 0;
    int* midList = (int*)malloc(sizeof(int) * size);
    midList[0] = array[mid];

    for (int i = 0; i < end + 1; i++){
        if (i == mid)
            continue;
        int element = array[i];
        if (element <= array[mid]){
            preList[preIndex] = element;
            preIndex++;
        }
        else{
            postList[postIndex] = element;
            postIndex++;
        }
    }

    if (preIndex == 0 && postIndex == 0){
        free(preList);

        free(postList);


        return midList;
    }
    else if (preIndex == 0){
        int* postSortedList = quickSort(postList, postIndex);
        memmove(&midList[1], &postSortedList[0], sizeof(postSortedList[0]) * postIndex);
        free(preList);

        free(postList);

        return midList;
    }
    else if (postIndex == 0){
        int* preSortedList = quickSort(preList, preIndex);
        memmove(&preSortedList[preIndex], &midList[0], sizeof(midList[0]));
        free(preList);

        free(postList);

        free(midList);

        return preSortedList;
    }
    else{
        int* preSortedList = quickSort(preList, preIndex);
        int* postSortedList = quickSort(postList, postIndex);
        memmove(&midList[1], &postSortedList[0], sizeof(postSortedList[0]) * postIndex);
        memmove(&preSortedList[preIndex], &midList[0], sizeof(midList[0]) * (postIndex + 1));

        free(preList);

        free(postList);

        free(midList);



        return preSortedList;
    }
}



int main(void)
{
    int* array = (int*)malloc(sizeof(int) * 10);
    array[0] = 5;
    array[1] = 9;
    array[2] = 4;
    array[3] = 2;
    array[4] = 7;
    array[5] = 6;
    array[6] = 1;
    array[7] = 3;
    array[8] = 8;
    array[9] = 10;

    array = quickSort(array, 10);

    printf("%d ", array[0]);
    printf("%d ", array[1]);
    printf("%d ", array[2]);
    printf("%d ", array[3]);
    printf("%d ", array[4]);
    printf("%d ", array[5]);
    printf("%d ", array[6]);
    printf("%d ", array[7]);
    printf("%d ", array[8]);
    printf("%d ", array[9]);

}
~~~

* Code Analysis
  * For extending of array, I use **memmove**

-----

* Procedure 2
  * Pick Pivot (First Element)
  * Left pointer moves from the second element until it reaches the right pointer
  * Right pointer moves from the last element until it reaches the left pointer
  * The element of left pointer whose value is bigger than pivot value is exchanged with the element of right pointer whose value is smaller than pivot value
  * When the left pointer meets the right pointer, pivot value (First element) is exchanged with the value whose next value is involved in values that are bigger than pivot
    * That is, the location where pivot value is going to move is the location of last element whose value is smaller than pivot value
  * {pre} pivot {post}
    * repeat the procedure for {pre}
    * repeat the procedure for {post}

~~~python
def quickSort(array, start, end):
    if start >= end:
        return

    pivot = start
    endOfArray = end
    pivot_value = array[pivot]
    start += 1

    while (start <= end):
        while (array[start] <= pivot_value and start < end):
            start += 1
        while (array[end] >= pivot_value and start <= end):
            end -= 1

        if start < end:
            swap(array, start, end)
        else:
            break

    swap(array, pivot, end)

    quickSort(array, pivot, end - 1)
    quickSort(array, end + 1, endOfArray)

def swap(array, index1, index2):
    temp = array[index1]
    array[index1] = array[index2]
    array[index2] = temp

array = [2, 3, 1, 8, 6, 7, 9]
quickSort(array, 0, len(array) - 1)
print(array)
~~~


-----

~~~java
import java.util.Arrays;
import java.util.ArrayList;

public class QuickSortImplementation {

	public static void main(String[] args) {
		Integer[] array = {2, 4, 3, 6, 5, 8, 10, 1, 7, 9};

		quickSort(array, 0, array.length - 1);

		System.out.println(new ArrayList(Arrays.asList(array)));

	}

	static <T extends Comparable<T>> void quickSort(T[] array, int left, int right) {
		if (left >= right)
			return;

		int pivot = left;
		int last = right;
		T value = array[pivot];
		int standard_index = 0;
		left++;


		while (left <= right) {
			while (array[left].compareTo(value) <= 0 && left < right)
				left++;
			while (array[right].compareTo(value) >= 0 && left <= right)
				right--;

			if (left < right)
				Swap(array, left, right);
			else
				break;
		}

		Swap(array, pivot, right);
		standard_index = right;

		if (standard_index == 0) {
			quickSort(array, standard_index + 1, last);
		}
		else if (standard_index == array.length - 1) {
			quickSort(array, pivot, standard_index - 1);
		}
		else {
			quickSort(array, pivot, standard_index - 1);
			quickSort(array, standard_index + 1, last);
		}


	}

	static <T> void Swap(T[] array, int data1_index, int data2_index) {
		T temp = array[data1_index];
		array[data1_index] = array[data2_index];
		array[data2_index] = temp;
	}

}
~~~

-----

~~~c
void swap(int* array, int index1, int index2)
{
    int temp = array[index1];
    array[index1] = array[index2];
    array[index2] = temp;
}



void quickSort(int* array, int start, int end)
{
    if (start >= end)
        return;

    int pivot = start;
    int endOfArray = end;
    int pivot_value = array[pivot];

    while (start <= end){
        while (array[start] <= pivot_value && start < end){
            start++;
        }
        while (array[end] >= pivot_value && start <= end){
            end--;
        }

        if (start < end)
            swap(array, start, end);
        else
            break;
    }

    swap(array, pivot, end);

    quickSort(array, pivot, end - 1);
    quickSort(array, end + 1, endOfArray);
}


int main(void)
{
    int* array = (int*)malloc(sizeof(int) * 10);
    array[0] = 5;
    array[1] = 9;
    array[2] = 4;
    array[3] = 2;
    array[4] = 7;
    array[5] = 6;
    array[6] = 1;
    array[7] = 3;
    array[8] = 8;
    array[9] = 10;

    quickSort(array, 0, 9);

    printf("%d ", array[0]);
    printf("%d ", array[1]);
    printf("%d ", array[2]);
    printf("%d ", array[3]);
    printf("%d ", array[4]);
    printf("%d ", array[5]);
    printf("%d ", array[6]);
    printf("%d ", array[7]);
    printf("%d ", array[8]);
    printf("%d ", array[9]);

    free(array);
    array = NULL;
}
~~~

### Time Complexity
* Time Complexity of Quick Sort = (the number of dividing steps) x (the number of comparison calculation per one dividing)
* Best Case: Everytime we divide the array, the pivot is the middle point
  * If we have 4 data, we only devide 2 times
  * If we have n data, we only devide log<sub>2</sub>n
  * The number of comparison calculation per one dividing step == the number of data (That is, all elements have to be inspected)
  * For the best case, we can expect its time complexity is nlogn
* Worst Case: Everytime we divide the array, the pivot is the first point (That is, **it is almost the sorted state**)
  * If we have 4 data, we need to devide 4 times
  * If we have n data, we need to devide n times
  * The number of comparison calculation per one dividing step == the number of data (That is, all elements have to be inspected)
  * For the worst case, we can expect its time complexity is n<sup>2</sup>
* Even if there is the worst case, quick sort is really efficient for the average
