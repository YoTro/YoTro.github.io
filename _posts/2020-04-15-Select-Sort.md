---
title: Select Sort
date: 2020-04-15 15:11:00
tags: [Sorting Algorithm]
music-id: 1309443665
youtube-video-ID: 
mathjax: true

---

# [Select sort](https://en.wikipedia.org/wiki/Selection_sort)

In computer science, selection sort is an in-place comparison sorting algorithm. It has an O(n2) time complexity, which makes it inefficient on large lists, and generally performs worse than the similar insertion sort. Selection sort is noted for its simplicity and has performance advantages over more complicated algorithms in certain situations, particularly where auxiliary memory is limited.

The algorithm divides the input list into two parts: a sorted sublist of items which is built up from left to right at the front (left) of the list and a sublist of the remaining unsorted items that occupy the rest of the list. Initially, the sorted sublist is empty and the unsorted sublist is the entire input list. The algorithm proceeds by finding the smallest (or largest, depending on sorting order) element in the unsorted sublist, exchanging (swapping) it with the leftmost unsorted element (putting it in sorted order), and moving the sublist boundaries one element to the right.

![Selectsort](//www.runoob.com/wp-content/uploads/2019/03/selectionSort.gif)

## Complexity

The time efficiency of selection sort is quadratic, so there are a number of sorting techniques which have better time complexity than selection sort. One thing which distinguishes selection sort from other sorting algorithms is that it makes the minimum possible number of swaps, n − 1 in the worst case.

| | Unstable Sorting Algorithm|
|:---:|:---:|
|Time|O(n^2)
|Space|O(1)|

## Source code

Check here [my github code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Select_sort.py)

# [选择排序](https://baike.baidu.com/item/%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F%E6%B3%95/2304587?fr=aladdin)

选择排序（Selection sort）是一种简单直观的排序算法。它的工作原理如下。首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

选择排序的主要优点与数据移动有关。如果某个元素位于正确的最终位置上，则它不会被移动。选择排序每次交换一对元素，它们当中至少有一个将被移到其最终位置上，因此对n个元素的表进行排序总共进行至多 n-1 次交换。在所有的完全依靠交换去移动元素的排序方法中，选择排序属于非常好的一种。

![选择排序](//www.runoob.com/wp-content/uploads/2019/03/selectionSort.gif)

## 时间空间复杂度

| |非稳定算法 |
|:---:|:---:|
|时间复杂度|O(n^2)
|空间复杂度|O(1)|


## 源码

点这里 [source code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Select_sort.py)

