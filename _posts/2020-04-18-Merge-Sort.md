---
title: Merge Sort
date: 2020-04-18 18:34:00
tags: [Sorting Algorithm]
music-id: 
youtube-video-ID: 
bilibili-video-ID: 
mathjax: true

---

# [Merge sort](https://en.wikipedia.org/wiki/Merge_sort)

Merge sort (also commonly spelled mergesort) is an efficient, general-purpose, comparison-based stable sorting algorithm. 
![loading...](https://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif  "mergesort")
![loading...](https://upload.wikimedia.org/wikipedia/commons/e/e6/Merge_sort_algorithm_diagram.svg  "mergesort")

## Complexity

| | |
|:---:|:--:|
| Time | $$O(N\log(N))$$ |

### [Numpy.sort](https://numpy.org/doc/stable/reference/generated/numpy.sort.html)

```python
def sort(a, axis=-1, kind=None, order=None):
    """
    Return a sorted copy of an array.
    Parameters
    ----------
    a : array_like
        Array to be sorted.
    axis : int or None, optional
        Axis along which to sort. If None, the array is flattened before
        sorting. The default is -1, which sorts along the last axis.
    kind : {'quicksort', 'mergesort', 'heapsort', 'stable'}, optional
        Sorting algorithm. The default is 'quicksort'. Note that both 'stable'
        and 'mergesort' use timsort or radix sort under the covers and, in general,
        the actual implementation will vary with data type. The 'mergesort' option
        is retained for backwards compatibility.
        .. versionchanged:: 1.15.0.
           The 'stable' option was added.
    order : str or list of str, optional
        When `a` is an array with fields defined, this argument specifies
        which fields to compare first, second, etc.  A single field can
        be specified as a string, and not all fields need be specified,
        but unspecified fields will still be used, in the order in which
        they come up in the dtype, to break ties.
    Returns
    -------
    sorted_array : ndarray
        Array of the same type and shape as `a`.

    =========== ======= ============= ============ ========
       kind      speed   worst case    work space   stable
    =========== ======= ============= ============ ========
    'quicksort'    1     O(n^2)            0          no
    'heapsort'     3     O(n*log(n))       0          no
    'mergesort'    2     O(n*log(n))      ~n/2        yes
    'timsort'      2     O(n*log(n))      ~n/2        yes
    =========== ======= ============= ============ ========
```

## [Source code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Merge_sort.py)

```python
#coding:UTF-8
#Author: Toryun
#Date: 2020-04-06 23:44:00
#Function: Merge Sort

import math
import time
import os
from random import shuffle

def Merge(left,right):
    t = 1
    res = []
    while t<10:
        if left[0]<right[0]:
            res.append(left.pop(0))
        elif left[0] == right[0]:
            res.append(left.pop(0))
            res.append(right.pop(0))
        else:
            res.append(right.pop(0))
        if len(left) == 0 and len(right) == 0:
            t = 10
        elif len(left) == 0 and len(right) > 0:
            res += right
            t = 10
        elif len(left)  > 0 and len(right) == 0:
            res += left
            t = 10
    #print res   
    return res

def Merge_sort(arr):
    if len(arr) <= 1:
        return arr
    else:
        m = len(arr)//2
        left = arr[:m]
        right = arr[m:]
        #print m,left,right
        left = Merge_sort(left)
        print left,right
        right = Merge_sort(right)
        print left,right
    return Merge(left, right)
                
if __name__ == '__main__':
    arr = [i for i in range(0,100)]
    shuffle(arr)
    print arr
    t0 = time.time()
    Merge_sort(arr)
    t1 = time.time()
    T = t1 - t0
    print "Merge Sort total time is {}".format(T)
```

## [Leetcode 378th kth-smallest-element-in-a-sorted-matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

```
Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

Example:

matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
Note:
You may assume k is always valid, 1 ≤ k ≤ n2.
```
The each of rows and columns are sorted in ascending order, so we could use merge sort algorithm to merge n arrays, use min-heap to pop kth min-value.
![loading...](https://pic.leetcode-cn.com/d550255f65fd12fb57130240046c176165c19689add0e817ccca5f88dc9340df-%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202020-07-02%20%E4%B8%8B%E5%8D%886.41.52.png, "min-heap")

## Solution

```c
#include <stdlib.h>
#include <stdio.h>
#include <stdbool.h>
typedef struct point{
    int val, x, y;

}point;

bool cmp(point a, point b){
    return a.val >= b.val;
}

void swap(point *a, point *b){
    point t = *a;
    *a = *b;
    *b = t;
}

void pop(point heap[], int* size){
    heap[1] = heap[(*size)--];
    int p = 1, s = 2;
    while (s <= *size){
        if (s < *size && !cmp(heap[s + 1], heap[s])){
            s++;
        }
        if (cmp(heap[s], heap[p])){
            break;
        }
        swap(&heap[s], &heap[p]);
        p = s;
        s = p << 1;
    }
}

void push(point heap[], int *size, point *p){
    /*把元素放入堆尾*/
    heap[++(*size)] = *p;
    int s = *size;
    while(s > 1){
        /*如果放入元素比父节点大,则不交换,否则一直交换到根节点*/
        if (cmp(heap[s], heap[s>>1])){
            break;
        }
        swap(&heap[s], &heap[s>>1]);
        s >>= 1;
    }
}
int kthSmall(int** matrix, int matrixSize, int k){
    /*创建最小根堆,此堆大小永远保持小于等于3*/
    point heap[matrixSize + 1];
    int size = 0;
    //把每行第一个元素入堆
    for (int i = 0; i < matrixSize; i++){
        point p = {matrix[i][0], i, 0};
        push(heap, &size, &p);
    }
    /*如题,第k个元素在已排序的数组里是第k-1个元素, 所以i必须小于k-1*/
    for (int i = 0; i < k - 1; i++){
        point now = heap[1];
        /*最小根出堆*/
        pop(heap, &size);
        if (now.y != matrixSize - 1){
            /*把堆中最小元素所在行的相邻元素入堆*/
            point p = {matrix[now.x][now.y + 1], now.x, now.y + 1};
            push(heap, &size, &p);
        }
    }
    /*返回第k个最小元素*/
    return heap[1].val;
}

int main(void){
    int n = 3;
    int **matrix = (int**)malloc(sizeof(int*)*n);
    for (int i = 0; i < n; i++){
        matrix[i] = (int*)malloc(sizeof(int)*n);
    }
    int a[3][3] = {\{1,5,9},{10,11,13},{12,13,15}\};
    for (int i = 0; i < n; i++){
        for (int j = 0; j < n; j++){
            matrix[i][j] = a[i][j];
            printf("%d, ", matrix[i][j]);
        }
        printf("\n");
    }
    int k = 8;
    int j = kthSmall(matrix, n, k);
    printf("%d ",j);
    for (int i = 0; i < n; i++){
        free(matrix[i]);
    }
    free(matrix);
    matrix = NULL;
    return 1;
}

```

## [Leetcode Reversepairs in arrays](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

```
Find the total number of reverse pairs in this array.

Note::
0 < = array length < = 50000

```
## [Solution](https://github.com/YoTro/CToryun/blob/master/Sorting/Inversion.c)

```c
//
//  Inversion.c
//  Function: get the inversion on array used by mergesort, regulartravel, tree
//
//  Created by Toryun on 2020/7/14.
//

#include <stdio.h>
#include <stdlib.h>
#include<time.h>
#define maxsize 10000
int c = 0; //compute inversion
int merge(long a[], int start, int mid, int end, long *ai){
    if(start >= end){
        return 0;
    }
    int coun = 0;
    int i = start, j = mid + 1, k = start;
    while (i <= mid && j <= end){
        if (a[i] <= a[j]){
            ai[k++] = a[i++];
        }else{
            c += mid - i + 1;
            coun += mid - i + 1;
            ai[k++] = a[j++];
        }
    }
    while (i <= mid){
        ai[k++] = a[i++];
    }
    while (j <= end){
        ai[k++] = a[j++];
    }
    for(int i = start; i <= end; i++){
        a[i] = ai[i];//copy sorted array to a
    }
    return coun;
}

int Mergesort(long a[], int start, int end, long *ai){
    if (start < end)
    {
        int mid = (start + end)/2;
        int i = Mergesort(a, start, mid, ai);
        int j = Mergesort(a, mid+1, end, ai);
        int c = merge(a, start, mid, end, ai); //merge all array
        return i+j+c;
    }
    return 0;
}

int regularInversion(long a[], int l){
    clock_t start = clock();
    int regularcount = 0;
    for (int i = 0; i < l - 1;i++){
        for (int j = i+1; j < l;j++){
            if (a[i] > a[j]){
                regularcount++;
                //printf("(%d, %d), ", a[i], a[j]);
            }
        }
    }
    printf("\n");
    clock_t end = clock();
    printf("Total used time of regulartravel: %f\n", (double)(end - start)/CLOCKS_PER_SEC);
    return regularcount;
}

long* randomlist(int n){
    long *a = (long*)malloc(sizeof(int)*maxsize);
    if(!a){
        printf("动态申请内存失败！\n"); exit(1); //异常退出
    }
    for (int i = 0; i < n;i++){
        a[i] = rand()%n + 1;//generate random number between 1~100
    }
    return a;
}
int main(void){
    int n = 100;
    //long p[] = {4,5,6,7,8,9};
    long *p = randomlist(n);
    long *ai = (long*)malloc(maxsize*sizeof(int));
    if(!ai){
        printf("动态申请内存失败！\n"); exit(1); //异常退出
    }
    printf("The size of array: %lu\n", sizeof(long));
    printf("The length of array: %d\n", n);
    int rcount = regularInversion(p, n);
    clock_t start = clock();
    int cont = Mergesort(p, 0, n-1, ai);
    clock_t end = clock();
    printf("Total used time of Mergesort: %f\n", (double)(end - start)/CLOCKS_PER_SEC);
    printf("Inversion is %d, %d, %d\n", c, cont, rcount);
    /*for (int i = 0; i <n;i++){
        printf("%d, ", ai[i]);
    }*/
    free(ai);
    free(p);
    return 1;
}

```
## My recent article about mergesort

[Timsort](https://yotro.github.io/2020/04/06/Timsort-Built-In-Python.html)

## Reference

1. [H.H. Goldstine and J. yon Neumarm, Planning and coding of problems for an electronic
computing instrument, Part 1I, Volume 2, reprinted in John yon Neumann Collected
Works, Volume V: Design o] Computers, Theory o] Automata and Numerical Analysis,
Pergamon Press, Oxford, England, 1963, pp. 152-214. ](https://library.ias.edu/files/pdfs/ecp/planningcodingof0103inst.pdf)

2. [D.E. Knuth, The Art of Computer Programming, Volume 3: Sorting and Searching,
Addison-Wesley Publishing Company, Reading, Mass., 1973. ](https://books.google.com/books?hl=en&lr=&id=cYULBAAAQBAJ&oi=fnd&pg=PR12&ots=KKEvOcJu5F&sig=kMNx9U1qg0GxVUThu3GcOf-Y--M#v=onepage&q=merge&f=false)
