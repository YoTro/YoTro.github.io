---
title: Radix sort
date: 2020-04-21 17:07:00
tags: [Sorting Algorithm]
music-id: 28283145
youtube-video-ID: 
bilibili-video-ID: 
mathjax: true

---

# Radix sort

[EN](#EN)|[CN](#CN)

![Radix sort](/images/Algorithm\ formula/Radix_sort.gif)

<span id="EN"> 

In computer science, radix sort is a non-comparative sorting algorithm. It avoids comparison by creating and distributing elements into buckets according to their radix. For elements with more than one significant digit, this bucketing process is repeated for each digit, while preserving the ordering of the prior step, until all digits have been considered. For this reason, radix sort has also been called bucket sort and digital sort.

Radix sort can be applied to data that can be sorted lexicographically, be they integers, words, punch cards, playing cards, or the mail.

## Complexity and performance

Radix sorts operates in O(nw) time, where n is the number of keys, and w is the key length. LSD variants can achieve a lower bound for w of 'average key length' when splitting variable length keys into groups as discussed above.

Optimized radix sorts can be very fast when working in a domain that suits them. They are constrained to lexicographic data, but for many practical applications this is not a limitation. Large key sizes can hinder LSD implementations when the induced number of passes becomes the bottleneck.

## Source code

[My Github code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Radix_sort.py)

<span id="CN"> 

# [基数排序](https://baike.baidu.com/item/%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F)

![Radix sort](/images/Algorithm\ formula/Radix_sort.gif)

一种非比较型整数排序算法，其原理是将整数按位数切割成不同的数字，然后按每个位数分别比较。由于整数也可以表达字符串（比如名字或日期）和特定格式的浮点数，所以基数排序也不是只能使用于整数。基数排序的发明可以追溯到1887年赫尔曼·何乐礼在打孔卡片制表机（Tabulation Machine）上的贡献。

它是这样实现的：将所有待比较数值（正整数）统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后，数列就变成一个有序序列。

基数排序的方式可以采用LSD（Least significant digital）或MSD（Most significant digital），LSD的排序方式由键值的最右边开始，而MSD则相反，由键值的最左边开始。

## 复杂度

Time O(nw)

## 源码

[My Github code](https://github.com/YoTro/Python_repository/blob/master/Sorting_Algorithms/Radix_sort.py)