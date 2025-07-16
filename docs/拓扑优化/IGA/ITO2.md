# Bézier分解算法
[论文地址](../source/等几何拓扑优化高效隐式过滤方法.pdf)

# 一、概述

无论是B样条还是NURBS隐式过滤方法，其基函数空间具有非单元一致性，每个参数单
元中心点处的基函数值及其索引均需计算和保存，降低了等几何拓扑优化的计算效率并增大了计算机的内存负担。

![alt text](../source/img/非单元一致.png)

Bernstein基函数具有单元局部基函数空间一致的特点，故该论文提出将NURBS基函数值的存储等价转换为单个单元Bernstein基函数值和所有参数方向Bézier提取矩阵的存储

---

# 二、Bézier分解算法

在节点矢量上不断插入节点，直到内部节点的重复度达到 𝑝−1,以此将 B 样条基函数拆分为 Bézier 基函数片段。

将节点 $\xi' \in [\xi_k, \xi_{k+1}]$ 插入 $\xi$ 得到新的节点区间 $\Xi' = \{\xi_1, \dots, \xi_k, \xi', \xi_{k+1}, \dots, \xi_{n+p+1}\}$

此时控制点更新规则如下：

$$P'_j =
\begin{cases}
P_1, & j=1 \\
\alpha_j P_j + (1-\alpha_j) P_{j-1}, & 1 < j < n+1 \\
P_n, & j=n+1
\end{cases} $$

其中 $\alpha_j$为权重因子,k为要插入的节点区间下标

$$\alpha_j =
\begin{cases}
1, & 1 \leq j \leq k-p \\
\frac{\xi' - \xi_j}{\xi_{j+p} - \xi_j}, & k-p+1 \leq j \leq k \\
0, & j \geq k+1
\end{cases} $$

**Bézier分解算法**

我们需要将内部节点的重复度变为p个，故不仅仅只插入一次，对于每次插入我们提取C矩阵（Bezier提取算子）

$$C_\text{ext}^w =
\begin{bmatrix}
\alpha_1 & 1-\alpha_1 & & \\
0 & \alpha_2 & & \\
\vdots & & \ddots & \\
0 & \cdots & \alpha_{n+w} & 1-\alpha_{n+w}
\end{bmatrix} $$

其规模为 $(n+w-1) \times (n+w)$ 因为每次控制点会变多，故列数会变多

多次插入即将这些算子相乘

$$C_\text{ext} = C_\text{ext}^1 \cdot C_\text{ext}^2 \cdots C_\text{ext}^w \cdots C_\text{ext}^m, \quad w=1,2,\dots,m$$

其规模为 $n\times(n+m)$ 

这里的算子表示的是B样条函数空间和Bernstein函数空间的映射

B样条函数空间如下：

$$N(\xi) =
\begin{Bmatrix}
N_{i,p}(\xi)
\end{Bmatrix}_{i=1}^n $$

Bernstein函数空间如下：

$$B(\xi) =
\begin{Bmatrix}
B_{i,p}(\xi)
\end{Bmatrix}_{i=1}^{n+m} $$

它们满足

$$N(\xi) = C_\text{ext} \cdot B(\xi) $$

Bezier控制点为新点，B样条控制点为旧点，控制点的映射关系如下：

$$\mathbf{P}_{\text{Bezier}} =
C_\text{ext}^{\mathrm{T}} \cdot
\mathbf{P}_{\text{Bspline}} $$

上式可以通过曲线形式不变证明

我们可以通过B样条和Bezier形式表示同一条曲线

$$
x(\xi) =
\sum_{i=1}^{n} N_{i,p}(\xi) P_{i}^{\text{Bspline}} =
\sum_{j=1}^{n+m} B_{j,p}(\xi) P_{j}^{\text{Bezier}} $$

使用矩阵形式表示更为简洁

$$x(\xi) =
N^\mathrm{T}(\xi) \cdot \mathbf{P}_{\text{Bspline}} =
B^\mathrm{T}(\xi) \cdot \mathbf{P}_{\text{Bezier}} $$

由于之前我们得出两个函数空间之间可以通过$C_{ext}$ Bezier算子进行转换

$$N(\xi) =
C_\text{ext} \cdot B(\xi) $$

故带入曲线表达式

$$x(\xi) =
\big( C_\text{ext} \cdot B(\xi) \big)^\mathrm{T} \cdot \mathbf{P}_{\text{Bspline}} =
B^\mathrm{T}(\xi) \cdot \big( C_\text{ext}^\mathrm{T} \cdot \mathbf{P}_{\text{Bspline}} \big) $$

根据曲线形式不变，可得(7)式

$$
\mathbf{P}_{\text{Bezier}} =
C_\text{ext}^\mathrm{T} \cdot \mathbf{P}_{\text{Bspline}} $$

---

# 三、高效隐式过滤方法

转化为Berstein基函数后，灵敏度计算如下：

$$\bar{x}_e (\alpha, \beta) =
\sum_{i=1}^r \sum_{j=1}^v R_{i,j} (\alpha, \beta) x_{i,j}^e =
\sum_{k=1}^{r \times v} \omega_{i,j}^e \cdot C_e^k \cdot B_{st} \cdot x_{i,j}^e =
\sum_{k=1}^{r \times v} \omega_{i,j}^e \cdot C_e^k \cdot B_{st} =
\frac{(x^e)^\mathrm{T} \cdot \big( \omega^e * (C_e \cdot B_{st}) \big)}{(\omega^e)^\mathrm{T} \cdot \big( C_e \cdot B_{st} \big)}
$$

* $\bar{x}^e = [\bar{x}_{1,1}^e, \cdots, \bar{x}_{r,v}^e]^\mathrm{T}$— 第 $e$ 个单元的控制点相对密度设计变量的集合。
* $x_{i,j}^e$ — 第 $e$ 个单元的第 $(i,j)$ 个控制点相对密度设计变量。
* $\omega^e = [\omega_{1,1}^e, \cdots, \omega_{r,v}^e]^\mathrm{T}$ — 第 $e$ 个单元相关的控制点的权重因子集合。
* $\omega_{i,j}^e$ — 第 $e$ 个单元相关的第 $(i,j)$ 个控制点对应的权重因子。
* 矩阵 $C_e$ — 单元 $e$ 的 Bézier 提取矩阵。
* $C_{ext}^{i,j}$ — 第 $(i,j)$ 个控制点对应的提取矩阵分量。
* $B_{st}$ — 任一单元相关的 Bernstein 基函数在单元中心点的函数值的集合。

对于单元的$C_{ext}$矩阵可以写成 $\alpha、\beta$方向的Bézier提取
矩阵的克罗内克（Kronecker）乘积，

$$
C_\text{ext} =
C_\text{ext}^\alpha \otimes C_\text{ext}^\beta
$$

!!! warning
    克罗内克乘积是矩阵的“按块扩展乘法”

$$
A =
\begin{bmatrix}
a_{11} & a_{12} \\\\
a_{21} & a_{22}
\end{bmatrix},
\quad
B =
\begin{bmatrix}
b_{11} & b_{12} \\\\
b_{21} & b_{22}
\end{bmatrix}
$$

那么它们的克罗内克积为：

$$
A \otimes B =
\begin{bmatrix}
a_{11} B & a_{12} B \\\\
a_{21} B & a_{22} B
\end{bmatrix}
$$

最后，灵敏度可计算如下：


$$
\frac{\partial C}{\partial x_{i,j}} =
\sum_{e \in W} \frac{\partial C}{\partial \bar{x}_{i,je}} \cdot \frac{\partial \bar{x}_{i,je}}{\partial x_{i,j}} =
\sum_{e \in W} \left[
\left(
\frac{\omega_{i,j}^e \cdot C_{e}^{i,j} \cdot B_{st}}{\omega_e^\mathrm{T} \cdot \big( C_e \cdot B_{st} \big) }
\right)
\cdot
\frac{\partial C}{\partial \bar{x}_{i,je}}
\right]
$$