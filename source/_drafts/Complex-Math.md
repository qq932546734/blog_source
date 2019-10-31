---
title: Complex Math
tags:
---

## 复数

复数的引入是为了简化相关的计算。复数是人为定义的。



### 复数的标准形式

$ z = a + ib$，其中$a$为复数的实部，$b$为复数的虚部，故$a = \Re[z], b = \Im[z]$

$i$为虚数，$i^2 = -1$

标准形式下，复数的加法和乘法为
$$
z_1 = a_1 + ib_1 \\\\
z_2 = a_2 + ib_2 \\\\
z_1 + z_2 = (a_1+a_2) + i(b_1+b_2) \\\\
z_1 z_2 = (a_1a_2 - b_1b_2) + i(a_1b_2 + a_2b_1)
$$

### 复数的共轭和模

复数$z$的共轭为$\overline{z} = a -ib$，其模为$||z|| = \sqrt{z\overline{z}} = \sqrt{a^2 + b^2}$

### 欧拉公式

也被称为万能公式，将虚数，三角函数以及自然底数完美结合到一个方程里面。
$$
e^{i\theta} = \cos \theta + i \sin \theta
$$

其推导过程是分别将等式的左右两边进行泰勒公式展开

### 复数的极坐标形式ploar form

上面我们看到复数的标准形式，
$$
z = a+ ib \\\\
\frac{z}{||z||} = \frac{a}{||z||} + i\frac{b}{||z||} = \frac{a}{\sqrt{a^2 + b^2}} + i\frac{b}{\sqrt{a^2 + b^2}}
$$
令

$$
\frac{a}{\sqrt{a^2 + b^2}} = \cos \theta \\\\
\frac{b}{\sqrt{a^2 + b^2}} = \sin \theta \\\\
\frac{z}{||z||} = \cos\theta + i\sin\theta = e^{i\theta} \\\\
$$
由此，我们可以推导出复数的极坐标形式：$z = ||z|| e^{i\theta}$

复数在极坐标上的位置，由复数的模$||z||$和其角度$\theta$决定。

### 极坐标下的除法

在标准坐标下，除法运算比较复杂，在极坐标下，
$$
\frac{z_1}{z_2} = \frac{||z_1||e^{i\theta_1}}{||z_2||e^{i\theta_2}} = \frac{||z_1||}{||z_2||}e^{i(\theta_1 - \theta_2)}
$$
