---
TIME_COMPLEXITIES-Learning
by Apocalypse from PKU
---



### 几种排序

汇总：

|          Name          |  Best   |  Average  | ***Worst*** | Memory | *Stable* |       Method        |                         Other notes                          |
| :--------------------: | :-----: | :-------: | :---------: | :----: | :------: | :-----------------: | :----------------------------------------------------------: |
|  In-place merge sort   |    —    |     —     |  $nlog^2n$  |   1    |   Yes    |       Merging       | Can be implemented as a stable sort based on stable in-place merging. |
|        Heapsort        | $nlogn$ |  $nlogn$  |   $nlogn$   |   1    |    No    |      Selection      |                                                              |
|   归并排序Merge sort   | $nlogn$ |  $nlogn$  |   $nlogn$   |  *n*   |   Yes    |       Merging       | Highly parallelizable (up to *O*(log *n*) using the Three Hungarian's Algorithm) |
|        Timsort         |   *n*   |  $nlogn$  |   $nlogn$   |  *n*   |   Yes    | Insertion & Merging | Makes *n-1* comparisons when the data is already sorted or reverse sorted. |
|     快排 Quicksort     | $nlogn$ |  $nlogn$  |    $n^2$    | $logn$ |    No    |    Partitioning     | Quicksort is usually done in-place with *O*(log *n*) stack space. |
|   希尔排序Shellsort    | $nlogn$ | $n^{4/3}$ |  $n^{3/2}$  |   1    |    No    |      Insertion      |                       Small code size.                       |
| 插入排序Insertion sort |   *n*   |   $n^2$   |    $n^2$    |   1    |   Yes    |      Insertion      | *O*(n + d), in the worst case over sequences that have *d* inversions. |
|  冒泡排序Bubble sort   |   *n*   |   $n^2$   |    $n^2$    |   1    |   Yes    |     Exchanging      |                       Tiny code size.                        |
| 选择排序Selection sort |  $n^2$  |   $n^2$   |    $n^2$    |   1    |    No    |                     |                                                              |

**1.插入排序Insertion Sort**

​	实现：

```python
def insertion_sort(arr):			
    for i in range(1, len(arr)):	
        j = i						
        while arr[j - 1] > arr[j] and j > 0:
            arr[j - 1], arr[j] = arr[j], arr[j - 1]
            j -= 1					
```

​	对每个数往前遍历（不断交换）直到插入合适位置

---

**2.冒泡排序Bubble Sort**

​	实现：

```python
def bubbleSort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if (swapped == False):
            break
```

​	n个从右到左递缩循环的从左到右的检查交换

------

**3.选择排序Selection Sort**

​	实现：

```python
def selectionSort(alist):
    for fillslot in range(len(alist)-1, 0, -1):
        positionOfMax = 0
        for location in range(1, fillslot+1):
            if alist[location] > alist[positionOfMax]:
                positionOfMax = location

        if positionOfMax != fillslot:
            alist[fillslot], alist[positionOfMax] = alist[positionOfMax], alist[fillslot]
```

​	重复从列表的未排序部分选择最值，放在已排序部分

​	有两种，一种从右到左选max，一种从左到右选min

---

**4.快速排序Quick Sort**

​	实现：（双指针法）（取右端点）

```python
def quicksort(arr, left, right): #分
    if left < right:
        partition_pos = partition(arr, left, right)
        quicksort(arr, left, partition_pos - 1)
        quicksort(arr, partition_pos + 1, right)

def partition(arr, left, right):#治：找pivot位置，且分开大小
    i = left
    j = right - 1
    pivot = arr[right]  #pivot 枢轴
    while i <= j:
        while i <= right and arr[i] < pivot:
            i += 1
        while j >= left and arr[j] >= pivot:
            j -= 1
        if i < j:
            arr[i], arr[j] = arr[j], arr[i]
    if arr[i] > pivot:  #对pivot的特判
        arr[i], arr[right] = arr[right], arr[i]
    return i

quicksort(arr, 0, len(arr) - 1)
```

​	quickSort 中的关键过程是分区（partition()）。分区的目的是将枢轴放置在排序数组中的正确位置，并将所有较小的元素放在枢轴的左边，将所有较大的元素放在枢轴的右边。

​	使用两个函数，一个用来分，一个用来治

​	注意选pivot方法有三种，左、右和三中位数（ len //2）

---

**5.合并排序Merge Sort**

​	实现：

```python
def mergeSort(arr):
	if len(arr) > 1:
		mid = len(arr)//2
		L = arr[:mid] # Dividing the array elements
		R = arr[mid:] # Into 2 halves
		mergeSort(L) # Sorting the first half
		mergeSort(R) # Sorting the second half

		i = j = k = 0
		# Copy data to temp arrays L[] and R[]
		while i < len(L) and j < len(R):
			if L[i] <= R[j]:
				arr[k] = L[i]
				i += 1
			else:
				arr[k] = R[j]
				j += 1
			k += 1
		# Checking if any element was left
		while i < len(L):
			arr[k] = L[i]
			i += 1
			k += 1
		while j < len(R):
			arr[k] = R[j]
			j += 1
			k += 1
```

​	不断分块直到单一元素，再递归拼接

---

笔试注意：

1.第n次**递归调用**函数时，函数被用了n+1次

---

**6.希尔排序Shell Sort：**

​	实现：

```py
def shellSort(arr, n):
    gap = n // 2
    while gap > 0:
        j = gap #设置步长
        while j < n:
            i = j - gap  # This will keep help in maintain gap value
            while i >= 0:
                # If value on right side is already greater than left side value
                # We don't do swap else we swap
                if arr[i + gap] > arr[i]:
                    break
                else:
                    arr[i + gap], arr[i] = arr[i], arr[i + gap]
                i = i - gap  # To check left side also
            # If the element present is greater than current element
            j += 1
        gap = gap // 2
```

​	希尔排序是直接插入排序算法的优化改进版本，通过增加步长来优化

---

