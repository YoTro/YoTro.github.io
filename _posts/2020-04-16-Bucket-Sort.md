---
title: Bucket Sort
date: 2020-04-16 12:56:00
tags: [Sorting Algorithm]
music-id: 461864856
youtube-video-ID: 
bilibili-video-ID: 
mathjax: true

---

# [Bucket sort](https://en.wikipedia.org/wiki/Bucket_sort)

Bucket sort, or bin sort, is a sorting algorithm that works by distributing the elements of an array into a number of buckets. Each bucket is then sorted individually, either using a different sorting algorithm, or by recursively applying the bucket sorting algorithm. It is a distribution sort, a generalization of pigeonhole sort, and is a cousin of radix sort in the most-to-least significant digit flavor. Bucket sort can be implemented with comparisons and therefore can also be considered a comparison sort algorithm. The computational complexity depends on the algorithm used to sort each bucket, the number of buckets to use, and whether the input is uniformly distributed.

Bucket sort works as follows:

1. Set up an array of initially empty "buckets".
2. Scatter: Go over the original array, putting each object in its bucket.
3. Sort each non-empty bucket.
4. Gather: Visit the buckets in order and put all elements back into the original array.

![bucketsort](//www.runoob.com/wp-content/uploads/2019/03/Bucket_sort_1.svg_.png)

![bucketsorted](//www.runoob.com/wp-content/uploads/2019/03/Bucket_sort_2.svg_.png)

## Source code

[My Github Code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Bucket_sort.py)

# [桶排序](https://baike.baidu.com/item/%E6%A1%B6%E6%8E%92%E5%BA%8F/4973777?fr=aladdin)

是一个排序算法，它的稳定性取决于你在桶内使用的排序算法。工作的原理是将数组分到有限数量的桶子里。每个桶子再个别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序）。桶排序是鸽巢排序的一种归纳结果。当要被排序的数组内的数值是均匀分配的时候，桶排序使用线性时间（Θ（n））。但桶排序并不是 比较排序，他不受到 O(n log n) 下限的影响。

![bucketsort](https://www.runoob.com/wp-content/uploads/2019/03/Bucket_sort_1.svg_.png)

![bucketsorted](//www.runoob.com/wp-content/uploads/2019/03/Bucket_sort_2.svg_.png)

1. 设置一个定量的数组当作空桶子。
2. 寻访序列，并且把项目一个一个放到对应的桶子去。
3. 对每个不是空的桶子进行排序。
4. 从不是空的桶子里把项目再放回原来的序列中。

## 源码

[Github Code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Bucket_sort.py)
