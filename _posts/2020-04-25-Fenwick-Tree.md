---
title: How to solve Inversion by Fenwick Tree
date: 2020-04-25 10:40:00
tags: [Graph,C,python,Tree]
music-id: 
youtube-video-ID: 
bilibili-video-ID: BV1pE41197Qj
mathjax: true
---

# [Fenwick tree](https://en.wikipedia.org/wiki/Fenwick_tree)

A Fenwick tree or binary indexed tree is a data structure that can efficiently update elements and calculate prefix sums in a table of numbers

## Complexity

|   Operation   | Time complexity |
| :-----------: | :-------------: |
|  add numbers  |    O(log n)     |
| query numbers |    O(log n)     |

## [Leetcode Finding Inversionpairs on array](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

```
Finding inversions in array, return numbers of them

Note:
0 <= size of array <= 50000
```
## [Solution](https://github.com/YoTro/CToryun/blob/master/Sorting/Inversion.c)

```c
//
//  Inversion.c
//  Function: get the inversion on array used by mergesort, regulartravel, Fenwick tree
//
//  Created by Toryun on 2020/7/14.
//

#include <stdio.h>
#include <stdlib.h>
#include<time.h>
#define maxsize 10001
int c = 0; //compute inversion
int BIT[maxsize];
int merge(int a[], int start, int mid, int end, int *ai){
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

int Mergesort(int a[], int start, int end, int *ai){
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

int regularInversion(int a[], int l){
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

int lowbit(int x){
    return x&-x;
}

void update(int x, int k){
    while(x < maxsize){
            BIT[x] += k;
            x += lowbit(x);
    }
}
int query(int x){
    int ans = 0;
    while (x > 0){
        ans += BIT[x];
        x -= lowbit(x);
    }
    return ans;
}

int cmp(const void *a, const void* b){
    return (*(int*)a)-(*(int*)b);
}
//binary search return the index in the array. key为要查找的数, p为要被查找的数组, 返回所在数组中的位置
int bindex(int key, int *p, int numsSize){
    int start = 0;
    int end = numsSize;
    int mid = start + (end - start)/2;
    while (start != end){
        if (p[mid] == key){
            return mid;
        }
        else if(p[mid] < key){
            start = mid + 1;
            mid = start + (end - start)/2;
        }
        else{
            end = mid;
            mid = start + (end - start)/2;
        }
    }
    return -1;
}
int reversePairs(int *nums, int numsSize){
    int ans = 0;
    //copy the array to tmp
    int *tmp = (int*)malloc(sizeof(int)*numsSize);
    for (int i = 0; i < numsSize;i++){
        tmp[i] = nums[i];
    }
    //sorting tmp
    qsort(tmp, numsSize, sizeof(int), cmp);
    //离散化(Discretization)数组
    for (int i = 0; i < numsSize; i++){
        nums[i] = bindex(nums[i], tmp, numsSize) + 1;
    }
    free(tmp);
    for (int i = numsSize - 1; i > -1 ;i--){
        ans += query(nums[i] - 1);
        update(nums[i], 1);
    }
    return ans;
}
//Randomly generate a range of arrays随机生成一定范围的数组
int* randomlist(int n){
    int *a = (int*)malloc(sizeof(int)*maxsize);
    if(!a){
        printf("动态申请内存失败！\n"); exit(1); //异常退出
    }
    for (int i = 0; i < n;i++){
        a[i] = rand()%(2*n + 1) - n;//generate random number between -n, n
    }
    return a;
}
//打印数组
void printlist(int *a, int numsSize){
    for(int i = 0; i < numsSize; i++ ){
        printf("%d, ", a[i]);
    }
    printf("\n");
}
int main(void){
    int n = 10000;
    //int p[] = {10,8,7,6,-5,10};
    int *p = randomlist(n);
    int *ai = (int*)malloc(maxsize*sizeof(int));
    if(!ai){
        printf("动态申请内存失败！\n"); exit(1); //异常退出
    }
    printf("The size of array: %d\n", n);
    int rcount = regularInversion(p, n);
    clock_t start1 = clock();
    int bitcount = reversePairs(p, n);
    clock_t end1 = clock();
    double t1 = (double)(end1 - start1)/CLOCKS_PER_SEC;
    clock_t start2 = clock();
    int cont = Mergesort(p, 0, n-1, ai);
    clock_t end2 = clock();
    double t2 = (double)(end2 - start2)/CLOCKS_PER_SEC;
    printf("Total used time of FenwickTree: %f\nTotal used time of Mergesort: %f\n", t1, t2);
    printf("Inversion is %d, %d, %d, %d\n", c, cont, rcount, bitcount);
    free(ai);
    free(p);
    return 1;
}

```
```python
class BIT:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)

    @staticmethod
    def lowbit(x):
        return x & (-x)
    
    def query(self, x):
        ret = 0
        while x > 0:
            ret += self.tree[x]
            x -= BIT.lowbit(x)
        return ret

    def update(self, x):
        while x <= self.n:
            self.tree[x] += 1
            x += BIT.lowbit(x)

class Solution():
    def reversePairs(self, nums):
        n = len(nums)
        # 离散化
        tmp = sorted(nums)
        for i in range(n):
            nums[i] = bisect.bisect_left(tmp, nums[i]) + 1
        # 树状数组统计逆序对
        bit = BIT(n)
        ans = 0
        for i in range(n - 1, -1, -1):
            ans += bit.query(nums[i] - 1)
            bit.update(nums[i])
        return ans

```

# Reference

1. [Peter M. Fenwick (1994). "A new data structure for cumulative frequency tables". Software: Practice and Experience. 24 (3): 327–336.](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.14.8917&rep=rep1&type=pdf)