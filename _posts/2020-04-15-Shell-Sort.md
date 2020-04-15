---
title: Shell Sort
date: 2020-04-14 10:26:00
tags: [Sorting Algorithm]
music-id: 
youtube-video-ID: eqrLWq-bRSc
mathjax: true

---

# [Shell sort](https://en.wikipedia.org/wiki/Shellsort)

Shellsort, also known as Shell sort or Shell's method, is an in-place comparison sort. It can be seen as either a generalization of sorting by exchange (bubble sort) or sorting by insertion (insertion sort). The method starts by sorting pairs of elements far apart from each other, then progressively reducing the gap between elements to be compared. Starting with far apart elements, it can move some out-of-place elements into position faster than a simple nearest neighbor exchange. Donald Shell published the first version of this sort in 1959. The running time of Shellsort is heavily dependent on the gap sequence it uses. For many practical variants, determining their time complexity remains an open problem.**It is a unstable sorting algorithm**.

![shellsort](//upload.wikimedia.org/wikipedia/commons/d/d8/Sorting_shellsort_anim.gif)


## Complexity

| | |
|:---:|:---:|
| Worst-case performance |O(n2) (worst known worst case gap sequence) O(n log2n) (best known worst case gap sequence|
| Best-case performance | O(n log n) (most gap sequences) O(n log2n) (best known worst-case gap sequence) |
|Average performance|depends on gap sequence|

## Source code

Check here [my github code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Shell_sort.py)

## [希尔排序](https://baike.baidu.com/item/%E5%B8%8C%E5%B0%94%E6%8E%92%E5%BA%8F)

希尔排序(Shell's Sort)是插入排序的一种又称“缩小增量排序”（Diminishing Increment Sort），是直接插入排序算法的一种更高效的改进版本。希尔排序是**非稳定排序算法**。该方法因D.L.Shell于1959年提出而得名。
希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，整个文件恰被分成一组，算法便终止。

希尔排序按其设计者希尔（Donald Shell）的名字命名，该算法由希尔1959年公布。一些老版本教科书和参考手册把该算法命名为Shell-Metzner，即包含Marlene Metzner Norton的名字，但是根据Metzner本人的说法，“我没有为这种算法做任何事，我的名字不应该出现在算法的名字中。”
希尔排序是基于插入排序的以下两点性质而提出改进方法的：
插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率。
但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位。

## 时间空间复杂度

| | |
|:---:|:---:|
| 最坏情况的性能| O（n2）（最坏的已知间隙间隔序列）O（n log2n）（最坏的已知间隙间隔序列|
| 最佳表现| O（n log n）（大多数间隔序列）O（n log2n）（最已知的最坏情况间隔序列）|
|平均性能|取决于间隙顺序|

## 匹配过程

<iframe style="width:100%;height="430" src="//player.bilibili.com/player.html?aid=285295963&bvid=BV19f4y1U7Hz&cid=178430290&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>


## 源码

点这里 [source code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Shell_sort.py)
