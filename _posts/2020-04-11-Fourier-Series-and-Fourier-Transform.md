---
title: Fourier Series and Fourier Transform
date: 2020-04-11 1:00:00
tags: [Fourier]
youtube-video-ID: r6sGWTCMz2k
mathjax: true

---

# [Fourier Series](https://en.wikipedia.org/wiki/Fourier_series)

[EN](#EN)|[CN](#CN)

<span id="EN">
## Definition

<iframe width="560" height="315" src="https://www.youtube.com/embed/spUNpyF58BY" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

## Formula

<div style="overflow:auto;">
\begin{equation} \begin{split} f(t)&=\frac{a_{0}}{2}+a_{1}cos(\omega t)+b_{1}sin(\omega t) \\ &+a_{2}cos(2\omega t)+b_{2}sin(2\omega t) \\ &+...\\ &=\frac{a_{0}}{2}+\sum_{n=1}^{\infty}{[a_{n}cos(n\omega t)+b_{n}sin(n\omega t)]} \end{split} \end{equation}
</div>

And the an ,bn
<div style="overflow:auto;">
\begin{aligned} a_{n}=\frac{2}{T}\int_{t_{0}}^{t_{0}+T}f(t)cos(n\omega t)dt \\ b_{n}=\frac{2}{T}\int_{t_{0}}^{t_{0}+T}f(t)sin(n\omega t)dt \end{aligned}
</div>

<span id="CN">

# [傅里叶级数](https://zh.wikipedia.org/wiki/%E5%82%85%E9%87%8C%E5%8F%B6%E7%BA%A7%E6%95%B0)

## 定义

在数学中，傅里叶级数（Fourier series，/ˈfʊrieɪ, -iər/）是把类似波的函数表示成简单正弦波的方式。更正式地说，它能将任何周期函数或周期信号分解成一个（可能由无穷个元素组成的）简单振荡函数的集合，即正弦函数和余弦函数（或者，等价地使用复指数）。离散时间傅里叶变换是一个周期函数，通常用定义傅里叶级数的项进行定义

<iframe src="//player.bilibili.com/player.html?aid=19141078&bvid=BV1pW411J7s8&cid=31220967&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## 公式

![fourier series](https://www.zhihu.com/equation?tex=%5Cbegin%7Bequation%7D+%5Cbegin%7Bsplit%7D+f%28t%29%26%3D%5Cfrac%7Ba_%7B0%7D%7D%7B2%7D%2Ba_%7B1%7Dcos%28%5Comega+t%29%2Bb_%7B1%7Dsin%28%5Comega+t%29+%5C%5C+%26%2Ba_%7B2%7Dcos%282%5Comega+t%29%2Bb_%7B2%7Dsin%282%5Comega+t%29+%5C%5C+%26%2B...%5C%5C+%26%3D%5Cfrac%7Ba_%7B0%7D%7D%7B2%7D%2B%5Csum_%7Bn%3D1%7D%5E%7B%5Cinfty%7D%7B%5Ba_%7Bn%7Dcos%28n%5Comega+t%29%2Bb_%7Bn%7Dsin%28n%5Comega+t%29%5D%7D+%5Cend%7Bsplit%7D+%5Cend%7Bequation%7D%5Ctag%7B1%7D)

![an_bn](https://www.zhihu.com/equation?tex=+%5Cbegin%7Balign%7D+%26a_%7Bn%7D%3D%5Cfrac%7B2%7D%7BT%7D%5Cint_%7Bt_%7B0%7D%7D%5E%7Bt_%7B0%7D%2BT%7Df%28t%29cos%28n%5Comega+t%29dt+%5Ctag%7B2%7D+%5C%5C+%26b_%7Bn%7D%3D%5Cfrac%7B2%7D%7BT%7D%5Cint_%7Bt_%7B0%7D%7D%5E%7Bt_%7B0%7D%2BT%7Df%28t%29sin%28n%5Comega+t%29dt+%5Ctag%7B3%7D%5C%5C+%5Cend%7Balign%7D)

![simple fourier series](//upload.wikimedia.org/wikipedia/commons/1/1a/Fourier_series_square_wave_circles_animation.gif)
