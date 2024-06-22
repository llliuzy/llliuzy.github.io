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

​	对每个数从左向右往前遍历（不断交换）直到插入合适位置

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

### 1. 树的定义

**定义一：树**由节点及连接节点的边构成。树有以下属性：
❏ 有一个根节点；
❏ 除根节点外，其他每个节点都与其唯一的父节点相连；
❏ 从根节点到其他每个节点都有且仅有一条路径；
❏ 如果每个节点最多有两个子节点，我们就称这样的树为二叉树。

**定义二：**一棵树要么为空，要么由一个根节点和零棵或多棵子树构成，子树本身也是一棵树。每棵子树的根节点通过一条边连到父树的根节点。图3展示了树的递归定义。从树的递归定义可知，图中的树至少有4个节点，因为三角形代表的子树必定有一个根节点。这棵树或许有更多的节点，但必须更深入地查看子树后才能确定。



### 2. 树的组成

**节点 Node**：节点是树的基础部分。
每个节点具有名称，或“键值”。节点还可以保存额外数据项，数据项根据不同的应用而变。

**边 Edge**：边是组成树的另一个基础部分。
每条边恰好连接两个节点，表示节点之间具有关联，边具有出入方向；
每个节点（除根节点）恰有一条来自另一节点的入边；
每个节点可以有零条/一条/多条连到其它节点的出边。<u>如果加限制不能有 “多条边”，这里树结构就特殊化为线性表</u>

**根节 Root**: 树中唯一没有入边的节点。

**路径 Path**：由边依次连接在一起的有序节点列表。比如，哺乳纲→食肉目→猫科→猫属→家猫就是一条路径。

**子节点 Children**：入边均来自于同一个节点的若干节点，称为这个节点的子节点。

**父节点 Parent**：一个节点是其所有出边连接节点的父节点。

**兄弟节点 Sibling**：具有同一父节点的节点之间为兄弟节点。

**子树 Subtree**：一个节点和其所有子孙节点，以及相关边的集合。

**叶节点 Leaf Node**：没有子节点的节点称为叶节点。

**层级 Level**：
从根节点开始到达一个节点的路径，所包含的边的数量，称为这个节点的层级。

**高度Height** ：所有节点层级的最大值。

**二叉树深度**：从根节点到叶节点依次经过的节点形成的树的一条路径，最长路径的节点个数为树的深度。

特别注意：根据定义，**depth=height+1**

### 3. 树的表示方法

（1）嵌套括号表示

例如：（A(D,E(F,G)),B,C）

（2）树形表示

（3）venn图表示

（4）凹入表

### 4. 树的遍历

原理：

前序：根、左子树、右子树（根左右）

中序：左子树、根、右子树（左根右）

后序：左子树、右子树、跟（右根左）

##### Kruskal算法

Kruskal算法是一种用于解决最小生成树（Minimum Spanning Tree，简称MST）问题的贪心算法。给定一个连通的带权无向图，Kruskal算法可以找到一个包含所有顶点的最小生成树，即包含所有顶点且边权重之和最小的树。

**以下是Kruskal算法的基本步骤：**

1. **将图中的所有边按照权重从小到大进行排序。**

2. **初始化一个空的边集，用于存储最小生成树的边。**

3. **重复以下步骤，直到边集中的边数等于顶点数减一或者所有边都已经考虑完毕：**

   - **选择排序后的边集中权重最小的边。**
   - **如果选择的边不会导致形成环路（即加入该边后，两个顶点不在同一个连通分量中），则将该边加入最小生成树的边集中。**

4. **返回最小生成树的边集作为结果。**

### 二、链表

#### 1、单链表

​	在链式结构中，除了要存储数据元素的信息外，还要存储它的后继元素的存储地址。

​	因此，为了表示**每个数据元素$$a_{i}$$与其直接后继元素$$a_{i+1}$$之间的逻辑关系，对数据$$a_{i}$$来说，除了存储其本身的信息之外，还需要存储一个指示其直接后继的信息（即直接后继的存储位置）。我们把存储数据元素信息的域称为数据域，把存储直接后继位置的域称为指针域。指针域中存储的信息称做指针或链。这两部分信息组成数据元素$$a_{i}$$的存储映像，称为结点（$$Node$$​）。**

​	我们把链表中第一个结点的存储位置叫做头指针。有时为了方便对对链表进行操作，会在单链表的第一个结点前附设一个节点，称为头结点，此时头指针指向的结点就是头结点。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210207165354972.png#pic_center)

​	空链表，头结点的直接后继为空。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210207165435359.png#pic_center)

```python
class LinkList:
    class Node:
        def __init__(self, data, next=None):
            self.data, self.next = data, next

    def __init__(self):
        self.head = self.tail = LinkList.Node(None, None)
        self.size = 0

    def print(self):
        ptr = self.head
        while ptr is not None:
            print(ptr.data, end=',')
            ptr = ptr.next

    def insert(self, p, data):
        nd = LinkList.Node(data, None)
        if self.tail is p:
            self.tail = nd
        nd.next = p.next
        p.next = nd
        self.size += 1

    def delete(self, p):
        if self.tail is p.next:
            self.tail = p
        p.next = p.next.next
        self.size -= 1

    def popFront(self):
        if self.head is None:
            raise Exception("Popping front for Empty link list.")
        else:
            self.head = self.head.next
            self.size -= 1
            if self.size == 0:
                self.head = self.tail = None

    def pushFront(self, data):
        nd = LinkList.Node(data, self.head)
        self.head = nd
        self.size += 1
        if self.size == 1:
            self.tail = nd

    def pushBack(self, data):
        if self.size == 0:
            self.pushFront(data)
        else:
            self.insert(self.tail, data)

    def clear(self):
        self.head = self.tail = None
        self.size = 0

    def __iter__(self):
        self.ptr = self.head
        return self

    def __next__(self):
        if self.ptr is None:
            raise StopIteration()
        else:
            data = self.ptr.data
            self.ptr = self.ptr.next
            return data

```





#### 2、双链表

​	**双向链表$$(Double$$ $$Linked$$ $$List)$$是在单链表的每个结点中，再设置一个指向其前驱结点的指针域。**所以在双向链表中的结点都有两个指针域，一个指向直接后继，另一个指向直接前驱。

```python
class DoubleLinkList:
    class Node:
        def __init__(self, data, prev=None, next=None):
            self.data, self.prev, self.next = data, prev, next

    class Iterator:
        def __init__(self, p):
            self.ptr = p

        def get(self):
            return self.ptr.data

        def set(self, data):
            self.ptr.data = data

        def __iter__(self):
            self.ptr = self.ptr.next
            if self.ptr is None:
                return None
            else:
                return DoubleLinkList.Iterator(self.ptr)

        def prev(self):
            self.ptr = self.ptr.prev
            return DoubleLinkList.Iterator(self.ptr)

    def __init__(self):
        self.head = self.tail = DoubleLinkList.Node(None, None, None)
        self.size = 0

    def _insert(self, p, data):
        nd = DoubleLinkList.Node(data, p, p.next)
        if self.tail is p:
            self.tail = nd
        if p.next:
            p.next.prev = nd
        p.next = nd
        self.size += 1

    def _delete(self, p):
        if self.size == 0 or p is self.head:
            return Exception("Illegal deleting.")
        else:
            p.prev.next = p.next
            if p.next:
                p.next.prev = p.prev
            if self.tail is p:
                self.tail = p.prev
            self.size -= 1

    def clear(self):
        self.tail = self.head
        self.head.next = self.head.prev = None
        self.size = 0

    def begin(self):
        return DoubleLinkList.Iterator(self.head.next)

    def end(self):
        return None

    def insert(self, i, data):
        self._insert(i.ptr, data)

    def delete(self, i):
        self._delete(i.ptr)

    def pushFront(self, data):
        self._insert(self.head, data)

    def pushBack(self, data):
        self._insert(self.tail, data)

    def popFront(self):
        self._delete(self.head.next)

    def popBack(self):
        self._delete(self.tail)

    def __iter__(self):
        self.ptr = self.head.next
        return self

    def __next__(self):
        if self.ptr is None:
            raise StopIteration()
        else:
            data = self.ptr.data
            self.ptr = self.ptr.next
            return data

    def find(self, val):
        ptr = self.head.next
        while ptr is not None:
            if ptr.data == val:
                return DoubleLinkList.Iterator(ptr)
            ptr = ptr.next
        return self.end()

    def printList(self):
        ptr = self.head.next
        while ptr is not None:
            print(ptr.data, end=',')
            ptr = ptr.next

```





#### 3、循环链表

​	**将单链表中终端节点的指针端由空指针改为指向头结点，就使整个单链表形成一个环，这种头尾相接的单链表称为单循环链表，简称循环链表。**

​	然而这样会导致访问最后一个结点时需要$$O(n)$$的时间，所以我们可以写出**仅设尾指针的循环链表**。

```python
class CircleLinkList:
    class Node:
        def __init__(self, data, next=None):
            self.data, self.next = data, next

    def __init__(self):
        self.tail = None
        self.size = 0

    def is_empty(self):
        return self.size == 0

    def pushFront(self, data):
        nd = CircleLinkList.Node(data)
        if self.is_empty():
            self.tail = nd
            nd.next = self.tail
        else:
            nd.next = self.tail.next
            self.tail.next = nd
        self.size += 1

    def pushBack(self, data):
        self.pushFront(data)
        self.tail = self.tail.next

    def popFront(self):
        if self.is_empty():
            return None
        else:
            nd = self.tail.next
            self.size -= 1
            if self.size == 0:
                self.tail = None
            else:
                self.tail.next = nd.next
        return nd.data

    def popBack(self):
        if self.is_empty():
            return None
        else:
            nd = self.tail.next
            while nd.next != self.tail:
                nd = nd.next
            data = self.tail.data
            nd.next = self.tail.next
            self.tail = nd
            return data

    def printList(self):
        if self.is_empty():
            print('Empty!')
        else:
            ptr = self.tail.next
            while True:
                print(ptr.data, end=',')
                if ptr == self.tail:
                    break
                ptr = ptr.next
            print()

```

