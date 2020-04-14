---
title: KMP Algorithm
date: 2020-04-14 7:59:00
tags: [Algorithm]
music-id: 
bilibili-video-ID: BV1Px411z7Yo
mathjax: true

---

# [KMP](https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm)

[EN](#EN)|[CN](#CN)

Knuth–Morris–Pratt algorithm

In computer science, the Knuth–Morris–Pratt string-searching algorithm (or KMP algorithm) searches for occurrences of a "word" W within a main "text string" S by employing the observation that when a mismatch occurs, the word itself embodies sufficient information to determine where the next match could begin, thus bypassing re-examination of previously matched characters.

The algorithm was conceived by James H. Morris and independently discovered by Donald Knuth "a few weeks later" from [automata theory](https://en.wikipedia.org/wiki/Finite-state_machine). Morris and Vaughan Pratt published a technical report in 1970. The three also published the algorithm jointly in 1977. Independently, in 1969, Matiyasevich discovered a similar algorithm, coded by a two-dimensional Turing machine, while studying a string-pattern-matching recognition problem over a binary alphabet. This was the first linear-time algorithm for string matching.

## Efficiency of the KMP algorithm

Since the two portions of the algorithm have, respectively, complexities of O(k) and O(n), the complexity of the overall algorithm is O(n + k).

These complexities are the same, no matter how many repetitive patterns are in W or S.

### KMP VS Linematch

<iframe width="560" height="315" src="https://www.youtube.com/embed/z-DEKOanlgI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

## Source code

Check here [KMP](https://github.com/YoTro/Python_repository/blob/master/Grapgh%20Theory/KMP.py)

<span id="CN">

# [KMP](https://baike.baidu.com/item/kmp%E7%AE%97%E6%B3%95/10951804?fromtitle=KMP&fromid=10158450&fr=aladdin)

## 定义

KMP算法是一种改进的字符串匹配算法，由D.E.Knuth，J.H.Morris和V.R.Pratt提出的，因此人们称它为克努特—莫里斯—普拉特操作（简称KMP算法）。KMP算法的核心是利用匹配失败后的信息，尽量减少模式串与主串的匹配次数以达到快速匹配的目的。具体实现就是通过一个next()函数实现，函数本身包含了模式串的局部匹配信息。KMP算法的时间复杂度O(m+n)。

### KMP算法的next［j］计算

<iframe style="width:100%;height:400" src="//player.bilibili.com/player.html?aid=70259921&bvid=BV1TJ411F7UZ&cid=121717870&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## KMP vs 线性排序

<iframe style="width:100%;height:400" src="//player.bilibili.com/player.html?aid=625281243&bvid=BV1pt4y127xz&cid=178105333&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## 源码

[Github code](https://github.com/YoTro/Python_repository/blob/master/Grapgh%20Theory/KMP.py)

## 参考

1. [有限状态机](https://baike.baidu.com/item/%E6%9C%89%E9%99%90%E7%8A%B6%E6%80%81%E8%87%AA%E5%8A%A8%E6%9C%BA?fromtitle=%E6%9C%89%E9%99%90%E7%8A%B6%E6%80%81%E6%9C%BA&fromid=2081914)