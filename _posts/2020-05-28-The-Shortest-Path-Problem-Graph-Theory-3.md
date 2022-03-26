---
title: Heap
date: 2020-05-28 19:59:00
tags: [Graph,C,Python]
music-id: 
youtube-video-ID: 
bilibili-video-ID: 

---

# [Heap](https://en.wikipedia.org/wiki/Heap_(data_structure))

[EN](#EN)|[CN](#CN)

<span id="EN"> 

In computer science, a heap is a specialized tree-based data structure which is essentially an almost complete tree that satisfies the heap property: in a max heap, for any given node C, if P is a parent node of C, then the key (the value) of P is greater than or equal to the key of C. In a min heap, the key of P is less than or equal to the key of C.[2] The node at the "top" of the heap (with no parents) is called the root node.

The heap is one maximally efficient implementation of an abstract data type called a priority queue, and in fact, priority queues are often referred to as "heaps", regardless of how they may be implemented. In a heap, the highest (or lowest) priority element is always stored at the root. However, a heap is not a sorted structure; it can be regarded as being partially ordered. A heap is a useful data structure when it is necessary to repeatedly remove the object with the highest (or lowest) priority.

A common implementation of a heap is the binary heap, in which the tree is a binary tree (see figure). The heap data structure, specifically the binary heap, was introduced by J. W. J. Williams in 1964, as a data structure for the heapsort sorting algorithm.[3] Heaps are also crucial in several efficient graph algorithms such as Dijkstra's algorithm. When a heap is a complete binary tree, it has a smallest possible height—a heap with N nodes and for each node a branches always has loga N height.

Note that, as shown in the graphic, there is no implied ordering between siblings or cousins and no implied sequence for an in-order traversal (as there would be in, e.g., a binary search tree). The heap relation mentioned above applies only between nodes and their parents, grandparents, etc. The maximum number of children each node can have depends on the type of heap.

Heaps are usually implemented with an implicit heap data structure, which is an implicit data structure consisting of an array (fixed size or dynamic array) where each element represents a tree node whose parent/children relationship is defined implicitly by their index. After an element is inserted into or deleted from a heap, the heap property may be violated and the heap must be balanced by swapping elements within the array.


Example of a complete binary max-heap with node keys being integers from 1 to 100 and how it would be stored in an array.
In an implicit heap data structure, the first (or last) element will contain the root. The next two elements of the array contain its children. The next four contain the four children of the two child nodes, etc. Thus the children of the node at position n would be at positions 2n and 2n + 1 in a one-based array, or 2n + 1 and 2n + 2 in a zero-based array. Computing the index of the parent node of n-th element is also straightforward. For one-based arrays the parent of element n is located at position n/2. Similarly, for zero-based arrays, is the parent is located at position (n-1)/2 (floored). This allows moving up or down the tree by doing simple index computations. Balancing a heap is done by sift-up or sift-down operations (swapping elements which are out of order). As we can build a heap from an array without requiring extra memory (for the nodes, for example), heapsort can be used to sort an array in-place.

Different types of heaps implement the operations in different ways, but notably, insertion is often done by adding the new element at the end of the heap in the first available free space. This will generally violate the heap property, and so the elements are then shifted up until the heap property has been reestablished. Similarly, deleting the root is done by removing the root and then putting the last element in the root and sifting down to rebalance. Thus replacing is done by deleting the root and putting the new element in the root and sifting down, avoiding a sifting up step compared to pop (sift down of last element) followed by push (sift up of new element).

Construction of a binary (or d-ary) heap out of a given array of elements may be performed in linear time using the classic Floyd algorithm, with the worst-case number of comparisons equal to 2N − 2s2(N) − e2(N) (for a binary heap), where s2(N) is the sum of all digits of the binary representation of N and e2(N) is the exponent of 2 in the prime factorization of N.[7] This is faster than a sequence of consecutive insertions into an originally empty heap, which is log-linear.

Python has a heapq module that implements a priority queue using a binary heap. The library exposes a heapreplace function to support k-way merging.

## [heapsort](https://en.wikipedia.org/wiki/Heapsort)

### Complexity
| | |
|: ---: |: --- :: |
| Average time complexity | (n log n) |
| Worst time complexity | O (n log n) |
| Optimal time complexity | O (n log n) |
| Space Complexity | O (n) total, O (1) auxiliary |
| Best solution | No |

see my [article](https://yotro.github.io/2020/04/17/Heap-Sort.html)

![heapsort](https://upload.wikimedia.org/wikipedia/commons/4/4d/Heapsort-example.gif)

[Source code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Heap_sort.py)

```python

#coding:utf-8
#Author: Toryun
#Date: 2020-06-10 2:00:00
#Function: 堆排序

import time
import numpy as np

class heap():
    '''堆类似于一个完全二叉树'''
    def __init__(self, arr):
        self.arr = arr
        self.size = len(arr)
    def swap(self,arr, i, j):
        '''交换函数'''
        arr[i], arr[j] = arr[j], arr[i]
    def heapify(self, arr, i):
        '''从堆顶开始进行比较,把最大值(或者最小值)放在父节点'''
        lchild = 2*i + 1 #初始化左节点(下标为1)
        rchild = 2*i + 2 #初始化右节点(下标为2)
        largestnode = i  #初始化父节点(下标为0)
        #如果左节点存在(在堆大小范围内),且左节点大于父节点,则临时最大根是左节点
        if lchild < self.size and arr[lchild] > arr[largestnode]:
            largestnode = lchild
        if rchild < self.size and arr[rchild] > arr[largestnode]:
            largestnode = rchild
        #如果进行了最大根下标改变了,则交换它们位置, 并递归地进行下一轮的heapify
        if largestnode != i:
            self.swap(arr, i, largestnode)
            self.heapify(arr, largestnode)
    def buildMaxHeap(self, arr):
        '''构建大根堆'''
        #从数组中间为根出发(向下取整)
        for i in range(int(self.size/2), -1, -1):
            self.heapify(arr, i)
    def MaxHeap(self):
        if self.size == 0:
            return []
        self.buildMaxHeap(self.arr)
        return self.arr

    def heapsort(self):
        '''
        堆排序
        :把最大值放在堆顶,然后和堆尾交换,self.size--来控制循环范围
        
        '''
        #构建大根堆
        self.arr = self.MaxHeap()
        print("构建大根堆:{}\n".format(self.arr))
        #进行深度遍历
        for i in range(self.size - 1, 0, -1):
            self.swap(self.arr, 0, i)
            self.size -= 1
            self.heapify(self.arr, 0)
        return self.arr

if __name__ == '__main__':
    arr = np.arange(50)
    np.random.shuffle(arr)
    print("初始化:{}\n".format(arr))
    t0 = time.time()
    Heap = heap(arr)
    arr = Heap.heapsort()
    t1 = time.time()
    print("排序后:{}\n".format(arr))
    T = t1 - t0
    print("Heapsort totoal time is {}s\n".format(T))

```
## Source code

Build heap
```c 
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
typedef int ElementType;//堆元素类型
typedef int LengthType; //堆的长度类型
/*定义堆*/
typedef struct
{
    ElementType* heap;//堆(数组)
    LengthType len; //堆的实时长度
    LengthType Maxsize;//用于保存初始化时所给的动态数组空间的大小,当堆满时可以扩大堆
}defheap;

//1. 初始化堆
void Init_Heap(defheap* heapq, ElementType m){
    if (m <= 0){
        printf("The size is less than 0, reinput the size of heap\n");
        exit(1);
    }
    /*给heap在堆区动态分配内存*/
    heapq->heap = (ElementType*)malloc(sizeof(ElementType)*m);
    if(!heapq->heap){
        printf("The memory is exhausted, please exit\n");
        exit(1);
    }
    heapq->len = 0;
    heapq->Maxsize = m;
    return;
}

//2. 清除堆
void Clear_Heap(defheap* heapq){
    if (heapq->heap != NULL){
        free(heapq->heap);
        heapq->len = 0;
        heapq->Maxsize = 0;
    }
}

//3. 检测堆是否为空
int isEmpty_Heap(defheap* heapq){
    if(!heapq->len){
        return 0;
    }else{
        return 1;
    }
}
//4. 插入堆,先添加到堆尾后移动x到对应的位置
void Insert_Heap(defheap* heapq, ElementType x){
    LengthType i;
    /*如果堆满了,就扩充堆尺寸*/
    if(heapq->len == heapq->Maxsize){
        ElementType* p = realloc(heapq->heap, 2*heapq->Maxsize*sizeof(ElementType));
        if(!p){
            printf("空间用完!\n");
            exit(1);
        }
        heapq->heap = p;
        heapq->Maxsize = 2*heapq->Maxsize;
    }
    /*向堆尾添加新元素x*/
    heapq->heap[heapq->len] = x;
    heapq->len++;
    i = heapq->len - 1;
    while(i != 0){
        //调整堆
        int j = (i - 1)/2;//i位置元素的父节点
        /*如果x大于i的父节点则退出*/
        if (x > heapq->heap[j]){
            break;
        }
        /*将父节点移动到i位置*/
        heapq->heap[i] = heapq->heap[j];
        i = j;

    }
    heapq->heap[i] = x;
}

//5. 删除堆顶元素

ElementType Delete_Heap(defheap* heapq){
    if(!heapq->len){
        printf("The heap is None\n");
        exit(1);
    }
    ElementType tmp, x;
    LengthType i, j;
    /*临时存储堆顶元素*/
    tmp = heapq->heap[0];
    heapq->len--;
    /*如果删除后堆空,则返回*/
    if (heapq->len == 0){
        return tmp;
    }
    x = heapq->heap[heapq->len];
    i = 0;
    j = 2*i + 1;//i节点的左孩子
    while(j <= heapq->len-1){
        /*如果存在右孩子且小于左孩子, j指向右孩子*/
        if (j < heapq->len-1 && heapq->heap[j] > heapq->heap[j+1]){
            j++;
        }
        /*如果x比j小,则退出*/
        if (x <= heapq->heap[j]){
            break;
        }
        /*否则把j孩子移到父节点*/
        heapq->heap[i] = heapq->heap[j];
        i = j;
        j = 2*i + 1;
    }
    heapq->heap[i] = x;
    return tmp;
}

int main(void)
{
    clock_t c0 = clock();
    int i, j, off = 0;
    int m = 10000000;
    int *a = (int*)malloc(sizeof(int)*m);
    ElementType x;
    srand((unsigned)time(NULL));
    defheap hp;
    Init_Heap(&hp, 5);
    for(i = 0; i < m; i++){
        a[i] = rand() % m + 1;
        Insert_Heap(&hp, a[i]);
    }
    while(isEmpty_Heap(&hp)){
        x = Delete_Heap(&hp);

        if(off){
            printf("%d", x);
            if(isEmpty_Heap(&hp)){
                printf(",");
            }
        }
    }
    Clear_Heap(&hp);
    clock_t c1 = clock();
    clock_t t = (c1 - c0)/CLOCKS_PER_SEC;
    printf("\nThe total time is %fs\n", (float)t);
    return 0;
}
```

<span id="CN"> 

# [Heap](https://en.wikipedia.org/wiki/Heap_(data_structure))

[EN](#EN)|[CN](#CN)

<span id="EN"> 

堆（英语：Heap）是计算机科学中的一种特别的树状数据结构。若是满足以下特性，即可称为堆：“给定堆中任意节点P和C，若P是C的母节点，那么P的值会小于等于（或大于等于）C的值”。若母节点的值恒小于等于子节点的值，此堆称为最小堆（min heap）；反之，若母节点的值恒大于等于子节点的值，此堆称为最大堆（max heap）。在堆中最顶端的那一个节点，称作根节点（root node），根节点本身没有母节点（parent node）。

> 堆结构即符合二分查找的时间特性,也符合链表的删除特点.

堆始于J. W. J. Williams在1964年发表的堆排序（heap sort），当时他提出了二叉堆树作为此算法的数据结构。堆在戴克斯特拉算法（英语：Dijkstra's algorithm）中亦为重要的关键。

在队列中，调度程序反复提取队列中第一个作业并运行，因为实际情况中某些时间较短的任务将等待很长时间才能结束，或者某些不短小，但具有重要性的作业，同样应当具有优先权。堆即为解决此类问题设计的一种数据结构


## 操作
	
| | |
|: ---: |: --- : |
|操作	|描述	|时间复杂度|
|build	|创建一个空堆	|O(n)|
|insert	|向堆中插入一个新元素	|O(log n)|
|update	|将新元素提升使其符合堆的性质|	|
|get	|获取当前堆顶元素的值	|O(1)|
|delete	|删除堆顶元素	|O(log n)|
|heapify|	使删除堆顶元素的堆再次成为堆|	|


## [堆排序](https://en.wikipedia.org/wiki/Heapsort)

### 复杂度
	
| | |
|:---:|:---:|
|平均时间复杂度| (nlog n)|
|最坏时间复杂度|	 O(nlog n)|
|最优时间复杂度|	 O(nlog n)|
|空间复杂度|	 O(n) total, O(1) auxiliary|
|最佳解|	不是|

see my [article](https://yotro.github.io/2020/04/17/Heap-Sort.html)

![heapsort](https://upload.wikimedia.org/wikipedia/commons/4/4d/Heapsort-example.gif)

[Source code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Heap_sort.py)

```python

#coding:utf-8
#Author: Toryun
#Date: 2020-06-10 2:00:00
#Function: 堆排序

import time
import numpy as np

class heap():
    '''堆类似于一个完全二叉树'''
    def __init__(self, arr):
        self.arr = arr
        self.size = len(arr)
    def swap(self,arr, i, j):
        '''交换函数'''
        arr[i], arr[j] = arr[j], arr[i]
    def heapify(self, arr, i):
        '''从堆顶开始进行比较,把最大值(或者最小值)放在父节点'''
        lchild = 2*i + 1 #初始化左节点(下标为1)
        rchild = 2*i + 2 #初始化右节点(下标为2)
        largestnode = i  #初始化父节点(下标为0)
        #如果左节点存在(在堆大小范围内),且左节点大于父节点,则临时最大根是左节点
        if lchild < self.size and arr[lchild] > arr[largestnode]:
            largestnode = lchild
        if rchild < self.size and arr[rchild] > arr[largestnode]:
            largestnode = rchild
        #如果进行了最大根下标改变了,则交换它们位置, 并递归地进行下一轮的heapify
        if largestnode != i:
            self.swap(arr, i, largestnode)
            self.heapify(arr, largestnode)
    def buildMaxHeap(self, arr):
        '''构建大根堆'''
        #从数组中间为根出发(向下取整)
        for i in range(int(self.size/2), -1, -1):
            self.heapify(arr, i)
    def MaxHeap(self):
        if self.size == 0:
            return []
        self.buildMaxHeap(self.arr)
        return self.arr

    def heapsort(self):
        '''
        堆排序
        :把最大值放在堆顶,然后和堆尾交换,self.size--来控制循环范围
        
        '''
        #构建大根堆
        self.arr = self.MaxHeap()
        print("构建大根堆:{}\n".format(self.arr))
        #进行深度遍历
        for i in range(self.size - 1, 0, -1):
            self.swap(self.arr, 0, i)
            self.size -= 1
            self.heapify(self.arr, 0)
        return self.arr

if __name__ == '__main__':
    arr = np.arange(50)
    np.random.shuffle(arr)
    print("初始化:{}\n".format(arr))
    t0 = time.time()
    Heap = heap(arr)
    arr = Heap.heapsort()
    t1 = time.time()
    print("排序后:{}\n".format(arr))
    T = t1 - t0
    print("Heapsort totoal time is {}s\n".format(T))

```
## Source code

Build heap
```c 
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
typedef int ElementType;//堆元素类型
typedef int LengthType; //堆的长度类型
/*定义堆*/
typedef struct
{
    ElementType* heap;//堆(数组)
    LengthType len; //堆的实时长度
    LengthType Maxsize;//用于保存初始化时所给的动态数组空间的大小,当堆满时可以扩大堆
}defheap;

//1. 初始化堆
void Init_Heap(defheap* heapq, ElementType m){
    if (m <= 0){
        printf("The size is less than 0, reinput the size of heap\n");
        exit(1);
    }
    /*给heap在堆区动态分配内存*/
    heapq->heap = (ElementType*)malloc(sizeof(ElementType)*m);
    if(!heapq->heap){
        printf("The memory is exhausted, please exit\n");
        exit(1);
    }
    heapq->len = 0;
    heapq->Maxsize = m;
    return;
}

//2. 清除堆
void Clear_Heap(defheap* heapq){
    if (heapq->heap != NULL){
        free(heapq->heap);
        heapq->len = 0;
        heapq->Maxsize = 0;
    }
}

//3. 检测堆是否为空
int isEmpty_Heap(defheap* heapq){
    if(!heapq->len){
        return 0;
    }else{
        return 1;
    }
}
//4. 插入堆,先添加到堆尾后移动x到对应的位置
void Insert_Heap(defheap* heapq, ElementType x){
    LengthType i;
    /*如果堆满了,就扩充堆尺寸*/
    if(heapq->len == heapq->Maxsize){
        ElementType* p = realloc(heapq->heap, 2*heapq->Maxsize*sizeof(ElementType));
        if(!p){
            printf("空间用完!\n");
            exit(1);
        }
        heapq->heap = p;
        heapq->Maxsize = 2*heapq->Maxsize;
    }
    /*向堆尾添加新元素x*/
    heapq->heap[heapq->len] = x;
    heapq->len++;
    i = heapq->len - 1;
    while(i != 0){
        //调整堆
        int j = (i - 1)/2;//i位置元素的父节点
        /*如果x大于i的父节点则退出*/
        if (x > heapq->heap[j]){
            break;
        }
        /*将父节点移动到i位置*/
        heapq->heap[i] = heapq->heap[j];
        i = j;

    }
    heapq->heap[i] = x;
}

//5. 删除堆顶元素

ElementType Delete_Heap(defheap* heapq){
    if(!heapq->len){
        printf("The heap is None\n");
        exit(1);
    }
    ElementType tmp, x;
    LengthType i, j;
    /*临时存储堆顶元素*/
    tmp = heapq->heap[0];
    heapq->len--;
    /*如果删除后堆空,则返回*/
    if (heapq->len == 0){
        return tmp;
    }
    x = heapq->heap[heapq->len];
    i = 0;
    j = 2*i + 1;//i节点的左孩子
    while(j <= heapq->len-1){
        /*如果存在右孩子且小于左孩子, j指向右孩子*/
        if (j < heapq->len-1 && heapq->heap[j] > heapq->heap[j+1]){
            j++;
        }
        /*如果x比j小,则退出*/
        if (x <= heapq->heap[j]){
            break;
        }
        /*否则把j孩子移到父节点*/
        heapq->heap[i] = heapq->heap[j];
        i = j;
        j = 2*i + 1;
    }
    heapq->heap[i] = x;
    return tmp;
}
//6. 返回堆顶元素
ElementType get_heap(defheap* heapq){
	if(!heapq->len){
		printf("The heap is empty\n");
		exit(1);
	}
	return heapq->heap[0];
}
//交换函数
void swap(int *a, int *b){
	int tmp = *b;
	*b = *a;
	*a = *b;
	return;
}
//7. heapify
void heapify(defheap* heapq, int i){
	/*如果大于堆的长度,返回*/
	if(i >= heapq->len){
		return;
	}
	int l, r, maxnode;
	l = 2*i + 1;//左节点下标(初始值为1)
	r = 2*i + 2;//右节点下标(初始值为2)
	maxnode = i;//父节点下标(最大值,初始值为0)
	/*如果左节点在堆范围内,且左节点大于最大值,交换下标*/
	if (l < heapq->len && heapq->heap[l] > heapq->heap[maxnode]){
		maxnode = l;
	}
	if (r < heapq->len && heapq->heap[r] > heapq->heap[maxnode]){
		maxnode = r;
	}
	/*如果发生了下标交换,则交换它们的值, 并递归地进行下一次heapify*/
	if (maxnode != i){
		swap(&heapq->heap[maxnode], &heapq->heap[i]);
		heapify(heapq, maxnode);
	}
}
int main(void)
{
    clock_t c0 = clock();
    int i, j, off = 0;
    int m = 10000000;
    int *a = (int*)malloc(sizeof(int)*m);
    ElementType x;
    srand((unsigned)time(NULL));
    defheap hp;
    Init_Heap(&hp, 5);
    for(i = 0; i < m; i++){
        a[i] = rand() % m + 1;
        Insert_Heap(&hp, a[i]);
    }
    while(isEmpty_Heap(&hp)){
        x = Delete_Heap(&hp);

        if(off){
            printf("%d", x);
            if(isEmpty_Heap(&hp)){
                printf(",");
            }
        }
    }
    Clear_Heap(&hp);
    clock_t c1 = clock();
    clock_t t = (c1 - c0)/CLOCKS_PER_SEC;
    printf("\nThe total time is %fs\n", (float)t);
    return 0;
}
```
