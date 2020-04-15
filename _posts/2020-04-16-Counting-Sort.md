---
title: Counting Sort
date: 2020-04-16 00:34:00
tags: [Sorting Algorithm]
music-id: 
youtube-video-ID: 
bilibili-video-ID: BV1fC4y1s7Wz
mathjax: true

---

# [Counting sort](https://en.wikipedia.org/wiki/Counting_sort)

In computer science, counting sort is an algorithm for sorting a collection of objects according to keys that are small integers; that is, it is an integer sorting algorithm. It operates by counting the number of objects that have each distinct key value, and using arithmetic on those counts to determine the positions of each key value in the output sequence. Its running time is linear in the number of items and the difference between the maximum and minimum key values, so it is only suitable for direct use in situations where the variation in keys is not significantly greater than the number of items. However, it is often used as a subroutine in another sorting algorithm, radix sort, that can handle larger keys more efficiently.

Because counting sort uses key values as indexes into an array, it is not a comparison sort, and the Ω(n log n) lower bound for comparison sorting does not apply to it. Bucket sort may be used for many of the same tasks as counting sort, with a similar time analysis; however, compared to counting sort, bucket sort requires linked lists, dynamic arrays or a large amount of preallocated memory to hold the sets of items within each bucket, whereas counting sort instead stores a single number (the count of items) per bucket. **It is a stable sorting algorithm**.

## Sorting algorithm

1. Find the largest element in the array to be sorted
2. Count the number of occurrences of each element in the array with value i
3. Accumulate all counts (start with the first element in arr_tmp, and add each item to the previous item)
4. Insert each element i in the arr[i] item of the new array arr_new, and subtract 1 for each arr[i]

## Source code

[Github code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/counting_sort.py)

# 计数排序

计数排序是一个非基于比较的**稳定的排序算法**，该算法于1954年由 Harold H. Seward 提出。它的优势在于在对一定范围内的整数排序时，它的复杂度为Ο(n+k)（其中k是整数的范围），快于任何比较排序算法.当然这是一种牺牲空间换取时间的做法，而且当O(k)>O(n*log(n))的时候其效率反而不如基于比较的排序（基于比较的排序的时间复杂度在理论上的下限是O(n*log(n)), 如归并排序，堆排序）

计数排序的核心在于将输入的数据值转化为键存储在额外开辟的数组空间中。作为一种线性时间复杂度的排序，计数排序要求输入的数据必须是有确定范围的整数。

由于用来计数的数组arr的长度取决于待排序数组中数据的范围（等于待排序数组的最大值与最小值的差加上1），这使得计数排序对于数据范围很大的数组，需要大量时间和内存。例如：计数排序是用来排序0到100之间的数字的最好的算法，但是它不适合按字母顺序排序人名。但是，计数排序可以用在基数排序中的算法来排序数据范围很大的数组。

## 算法的步骤如下：

1. 找出待排序的数组中最大元素
2. 统计数组中每个值为i的元素出现的次数，存入数组arr_tmp的第i项
3. 对所有的计数累加（从arr_tmp中的第一个元素开始，每一项和前一项相加）
4. 倒序地将每个元素i放在新数组arr_new的第arr[i]项，每放一个元素就将arr[i]减去1【**倒序可以保持算法的稳定性**】

## 源码

[Github code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/counting_sort.py)
