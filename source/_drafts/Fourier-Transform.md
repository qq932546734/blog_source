---
title: Fourier Transform
tags:
---

### 学习之前的疑惑
1. 傅里叶变换只能用来转换周期函数，但wavform之类的函数，并不是周期函数啊？任何周期函数都可以用不同周期的三角函数进行拟合。
2. 傅里叶级数（Fourier Series）和傅里叶变换（Fourier Transform）是不同的，前者用于周期函数的展开；后者用于非周期函数的变换？


### 基础知识
对于$y = A \sin(wx + C) + D$， 其中周期等于$\frac{2\pi}{w}$，频率等于周期的倒数$\frac{w}{2\pi}$，振幅是A。

### 傅里叶级数

假设$f(x)$为周期为T的函数，并满足傅里叶级数的收敛条件，其傅里叶级数可写为：
$$
f(x) = \frac{a_0}{2} + \sum_{n=1}^{\infty}(a_n\cos {\frac{2\pi nx}{T}} + b_n \sin {\frac{2\pi nx}{T}}) \\\\
a_n = \frac{2}{T} \int_{x_0}^{x_0 + T} f(x)\cos {\frac{2\pi nx}{T}} dx \\\\
b_n = \frac{2}{T}\int_{x_0}^{x_0+T} f(x) \cos{\frac{2\pi nx}{T}}dx
$$




### reference

1. [傅里叶专题分析]( [https://ccjou.wordpress.com/%E5%B0%88%E9%A1%8C%E6%8E%A2%E7%A9%B6/%E5%82%85%E7%AB%8B%E8%91%89%E5%88%86%E6%9E%90%E5%B0%88%E9%A1%8C/](https://ccjou.wordpress.com/專題探究/傅立葉分析專題/) )