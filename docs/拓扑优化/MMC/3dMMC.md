## 概述


## 方法

### 数学表达式

对于最小柔度设计问题，有限元离散化下的数学表述为：

$$ \text{find}\,d=((d^{1})^{T},...,(d^{n})^{T})^{T}\in\mathcal{U}^{d} $$

$$ \min\,f=F^{T}U $$

$$ \text{s.t.}\,KU=F $$

$$ g=V/|D|-v\leq 0 $$

其中$K$、$F$和$U$分别是全局刚度矩阵、节点力向量和节点位移向量；$V$、$|D|$、$v$分别是优化结构的体积、设计域的体积、允许体积分数的上限，$\mathcal{U}^{d}$表示设计变量的可行域。

### 组件描述

- 每个组件为一个长方体，由9个参数描述：
    - 中心坐标：$(x_0, y_0, z_0)$
    - 半长宽高：$(L_1, L_2, L_3)$
    - 旋转角：$(\alpha, \beta, \gamma)$
    - 局部坐标系下TDF公式为：
  
$$
\varphi_i = 1 - \left[ \left( \frac{x^\prime}{L_{i1}} \right)^p + \left( \frac{y^\prime}{L_{i2}} \right)^p + \left( \frac{z^\prime}{L_{i3}} \right)^p \right]^{1/p} \tag{1}
$$

- 通过坐标变换（欧拉角）将局部坐标映射到全局坐标。

### TDF

TDF 是 Topology Description Function 的缩写，指拓扑描述函数，解释如下：

**局部TDF：**

每个单元的物理属性（如密度、刚度）由一个拓扑描述函数（TDF）决定。

对于设计域 \( D \) 中的一个点 \( x \) 和第 \( i \) 个组件 \( \Omega_i \)：

- 当 \( \varphi_i(x) > 0 \) 时，表示点 \( x \) 位于该组件的**内部**；
- 当 \( \varphi_i(x) = 0 \) 时，表示点 \( x \) 位于该组件的**边界上**；
- 当 \( \varphi_i(x) < 0 \) 时，表示点 \( x \) 位于该组件的**外部**。

**全局TDF：**

只要一个点在任何一个组件内部$\phi_i>0$，它就在整体结构内部，故定义如下：

全局TDF = $\max(\varphi_1,\varphi_2,\dots,\varphi_n)$

由于$\max$函数在交界处是不可导的（比如两个组件相交的地方，函数有"尖角"），而优化算法（如梯度下降、MMA）需要导数来知道"往哪个方向调整设计变量能让目标更好"，故使用K-S函数来近似$\max$

K-S函数提供了一个光滑的、可导的近似来代替$\max$：

$$
\phi^s=\frac{1}{\lambda}\ln\left(\sum_{k=1}^n\exp\left(\lambda\phi^k\right)\right) \tag{2}
$$

其中：

- $\phi^s$：近似的"最大值"（全局TDF）
- $\lambda$：一个很大的正数（如50,100,200），称为惩罚因子或光滑参数
- $\exp(\lambda\phi^k)$：指数函数放大差异


### 杨氏模量计算


第$e$个有限元的杨氏模量$E_{e}$和密度$\rho_{e}$计算如下：

$$ E_{e}=\rho_{e}E\text{ and}\rho_{e}=\frac{1}{8}\sum_{i=1}^{8}H_{\epsilon}^{\alpha}(\phi_{e,i}^{s}) \tag{3} $$

其中$E$是实体材料的杨氏模量，$\phi_{e,i}^{s}$ 是全局TDF $\phi^{s}$在第$e$个单元的第$i$个节点处的值。符号$H_{\epsilon}^{\alpha}$表示平滑的Heaviside函数。

$$ H_{\epsilon}^{\alpha}(x)=\begin{cases}1,&\text{if}\,x>\epsilon\\\frac{3(1-\alpha)}{4}(\frac{x}{\epsilon}-\frac{x^{3}}{3\epsilon^{3}})+\frac{1+\alpha}{2},&\text{if}\,|x|\leq\epsilon\\\alpha,&\text{otherwise}\end{cases} \tag{4}$$

其中：$\epsilon$为控制过渡区宽度的参数，$\alpha$为一个小的正数，用于防止当TDF接近0时，$ρ_e$过小导致刚度矩阵奇异

!!! warning
    $\epsilon$AI解释：在优化过程中，我们需要计算目标函数对设计变量的导数（灵敏度），以指导搜索方向。如果函数不可导，就无法使用高效的梯度优化算法（如MMA）。过渡区：指的是函数值从接近 α 变化到接近 1 所覆盖的 φ 值范围，过渡区左侧，TDF对应密度值稳定再$\alpha$，右侧稳定在1，中间即为过渡区，函数值从$\alpha$平滑上升到1

### 敏感性分析

**目标函数灵敏度**

$$
\frac{\partial f}{\partial d_{j}^{i}}=\sum_{e=1}^{N}\frac{\partial f}{\partial\rho_{e}}\frac{\partial\rho_{e}}{\partial d_{j}^{i}}=-\sum_{e=1}^{N}u_{e}^{\top}k_{e}^{0}u_{e}\frac{\partial\rho_{e}}{\partial\phi^{s}}\frac{\partial\phi^{i}}{\partial\phi^{i}}\frac{\partial\phi^{s}}{\partial d_{j}^{i}} \tag{5}
$$

推导如下：

$$

$$
首先对目标函数求导

$$\frac{\partial f}{\partial d_{j}^{i}}=\mathbf{F}^{T}\frac{\partial\mathbf{U}}{\partial d_{j}^{i}} \tag{6}$$

但$\mathbf{U}$受到刚度矩阵$\mathbf{K}$的影响。根据有限元平衡方程$\mathbf{K}\mathbf{U}=\mathbf{F}$，使用伴随法来计算$\frac{\partial\mathbf{U}}{\partial d_{j}^{i}}$。

对平衡方程两边同时对$d_{j}^{i}$求导：

$$ \frac{\partial\mathbf{K}}{\partial d_{j}^{i}}\mathbf{U}+\mathbf{K}\frac{\partial\mathbf{U}}{\partial d_{j}^{i}}=\frac{\partial\mathbf{F}}{\partial d_{j}^{i}} \tag{7}$$

假设载荷$\mathbf{F}$与设计无关，则右边为0：

$$ \frac{\partial\mathbf{K}}{\partial d_{j}^{i}}\mathbf{U}+\mathbf{K}\frac{\partial\mathbf{U}}{\partial d_{j}^{i}}=0 \tag{8}$$

引入一个伴随变量（即拉格朗日乘子）$\boldsymbol{\lambda}$，令其满足：
$$ \mathbf{K}\boldsymbol{\lambda}=\mathbf{F} \tag{9}$$

然后将上式左乘$\boldsymbol{\lambda}^{T}$：

$$ \boldsymbol{\lambda}^{T}\frac{\partial\mathbf{K}}{\partial d_{j}^{i}}\mathbf{U}+\boldsymbol{\lambda}^{T}\mathbf{K}\frac{\partial\mathbf{U}}{\partial d_{j}^{i}}=0 \tag{10}$$

利用$\mathbf{K}\boldsymbol{\lambda}=\mathbf{F}$，有$\boldsymbol{\lambda}^{T}\mathbf{K}=\mathbf{F}^{T}$，所以：

$$ \boldsymbol{\lambda}^{T}\frac{\partial\mathbf{K}}{\partial d_{j}^{i}}\mathbf{U}+\mathbf{F}^{T}\frac{\partial\mathbf{U}}{\partial d_{j}^{i}}=0 \tag{11}$$

而$\mathbf{F}^{T}\frac{\partial\mathbf{U}}{\partial d_{j}^{i}}=\frac{\partial f}{\partial d_{j}^{i}}$，因此：

$$ \frac{\partial f}{\partial d_{j}^{i}}=-\boldsymbol{\lambda}^{T}\frac{\partial\mathbf{K}}{\partial d_{j}^{i}}\mathbf{U} \tag{12}$$

注意到$\lambda=U$（因为$KU=F$，且K对称），所以：

$$ \frac{\partial f}{\partial d_{j}^{i}}=-U^{T}\frac{\partial K}{\partial d_{j}^{i}}U  \tag{13}$$

**接下来求解$\frac{\partial K}{\partial d_{j}^{i}}$**

刚度矩阵$\mathbf{K}$是所有单元刚度矩阵的组装结果。每个单元的刚度矩阵$\mathbf{k}_{e}$依赖于其材料属性$\rho_{e}$。

所以：

$$ \frac{\partial\mathbf{K}}{\partial d_{j}^{i}}=\sum_{e=1}^{N}\frac{\partial\mathbf{k}_{e}}{\partial d_{j}^{i}} \tag{14}$$

而$\mathbf{k}_{e}=\rho_{e}\mathbf{k}_{e}^{0}$，其中$\mathbf{k}_{e}^{0}$是固体材料的刚度矩阵。

因此：

$$ \frac{\partial\mathbf{k}_{e}}{\partial d_{j}^{i}}=\frac{\partial\rho_{e}}{\partial d_{j}^{i}}\mathbf{k}_{e}^{0} \tag{15}$$

代入(13)式：

$$ \frac{\partial f}{\partial d_{j}^{i}}=-\sum_{e=1}^{N}\mathbf{U}_{e}^{T}\left(\frac{\partial\rho_{e}}{\partial d_{j}^{i}}\mathbf{k}_{e}^{0}\right)\mathbf{U}_{e}=-\sum_{e=1}^{N}\frac{\partial\rho_{e}}{\partial d_{j}^{i}}\cdot\mathbf{u}_{e}^{T}\mathbf{k}_{e}^{0}\mathbf{u}_{e} \tag{16}$$

这正是公式(5)的第一部分：

$$ \frac{\partial f}{\partial d_{j}^{i}}=\sum_{e=1}^{N}\frac{\partial f}{\partial\rho_{e}}\frac{\partial\rho_{e}}{\partial d_{j}^{i}}=-\sum_{e=1}^{N}\mathbf{u}_{e}^{T}\mathbf{k}_{e}^{0}\mathbf{u}_{e}\cdot\frac{\partial\rho_{e}}{\partial d_{j}^{i}} \tag{17}$$

其中$\frac{\partial f}{\partial\rho_{e}}=-\mathbf{u}_{e}^{T}\mathbf{k}_{e}^{0}\mathbf{u}_{e}$是"应力相关项"。


**约束函数灵敏度**

$$
\frac{\partial g}{\partial d_{j}^{i}}=\sum_{e=1}^{N}\frac{\partial g}{\partial\rho_{e}}\frac{\partial\rho_{e}}{\partial d_{j}^{i}}=\sum_{e=1}^{N}\frac{V_{e}}{|D|}\frac{\partial\rho_{e}}{\partial\phi^{s}}\frac{\partial\phi^{s}}{\partial\phi^{i}}\frac{\partial\phi^{i}}{\partial d_{j}^{i}} \tag{18}
$$

推导如下：

约束函数:

$$ g=\frac{V}{|D|}-v=\frac{1}{|D|}\sum_{e=1}^{N}V_{e}\rho_{e}-v \tag{19}$$

对$d_{j}^{i}$求导:

$$ \frac{\partial g}{\partial d_{j}^{i}}=\frac{1}{|D|}\sum_{e=1}^{N}V_{e}\frac{\partial\rho_{e}}{\partial d_{j}^{i}}    \tag{20}$$

这正是公式(18)的第一部分:

$$ \frac{\partial g}{\partial d_{j}^{i}}=\sum_{e=1}^{N}\frac{\partial g}{\partial\rho_{e}}\frac{\partial\rho_{e}}{\partial d_{j}^{i}}=\sum_{e=1}^{N}\frac{V_{e}}{|D|}\frac{\partial\rho_{e}}{\partial d_{j}^{i}} \tag{21}$$


**链式法则中间过程**

$$
\frac{\partial\rho_{e}}{\partial\phi^{s}}\frac{\partial\phi^{s}}{\partial\phi^{i}}=\frac{1}{8}\sum_{j=1}^{8}H_{e}^{\alpha^{\prime}}(\phi_{e,j}^{s})\frac{\exp(\lambda\phi_{e,j}^{i})}{\sum_{k=1}^{n}\exp(\lambda\phi_{e,j}^{k})}\qquad(22)
$$

推导如下：

$$ \frac{\partial\rho_{e}}{\partial d_{j}^{i}}=\frac{1}{8}\sum_{i=1}^{8}\frac{\partial H^{\alpha}\left(\phi_{e,i}^{s}\right)}{\partial\phi_{e,i}^{s}}\cdot\frac{\partial\phi_{e,i}^{s}}{\partial d_{j}^{i}} \tag{23}$$

而$\frac{\partial H^{\alpha}}{\partial\phi}$是平滑Heaviside函数的导数，即其斜率。

根据公式(4)：

$$ H^{\alpha}(x)=\begin{cases}1,&x>\varepsilon\\\frac{3(1-\alpha)}{4}\left(\frac{x}{\varepsilon}-\frac{x^{3}}{3\varepsilon^{3}}\right)+\frac{1}{2},&|x|\leq\varepsilon\\\alpha,&x<-\varepsilon\end{cases} \tag{24}$$

对其求导：

$$ \frac{dH^{\alpha}}{dx}=\left\{\begin{array}{ll}0,&x>\varepsilon\\\frac{3(1-\alpha)}{4}\left(\frac{1}{\varepsilon}-\frac{x^{2}}{\varepsilon^{3}}\right),&|x|\leq\varepsilon\\0,&x<-\varepsilon\end{array}\right. \tag{25}$$

在论文中，这个导数被写成：

$$ \frac{\partial H^{\alpha}}{\partial\phi}=H_{\varepsilon}^{\prime}\left(\phi_{e,i}^{s}\right) \tag{26}$$

因此：

$$ \frac{\partial\rho_{e}}{\partial d_{j}^{i}}=\frac{1}{8}\sum_{i=1}^{8}H_{\varepsilon}^{\prime}\left(\phi_{e,i}^{s}\right)\cdot\frac{\partial\phi_{e,i}^{s}}{\partial d_{j}^{i}} \tag{27}$$

再根据公式(2)K-S函数近似

$$ \phi^{s}=\max_{i}\phi^{i}\approx\mathrm{K}-\mathrm{S}\text{ function } \tag{28}$$

对某个组件i的参数$d_{j}^{i}$求导时，只有当该组件的TDF$\phi^{i}$是当前最大值时，它才对总TDF有贡献。因此：

$$ \frac{\partial\phi^{s}}{\partial d_{j}^{i}}=\frac{\exp\left(\lambda\phi^{i}\right)}{\sum_{k=1}^{n}\exp\left(\lambda\phi^{k}\right)}\cdot\frac{\partial\phi^{i}}{\partial d_{j}^{i}} \tag{29}$$

其中λ是K-S函数中的惩罚因子，用于逼近最大值。

所以：

$$ \frac{\partial\phi_{e,i}^{s}}{\partial d_{j}^{i}}=\frac{\partial\phi^{s}}{\partial d_{j}^{i}}=\frac{\exp\left(\lambda\phi^{i}\right)}{\sum_{k=1}^{n}\exp\left(\lambda\phi^{k}\right)}\cdot\frac{\partial\phi^{i}}{\partial d_{j}^{i}} \tag{30}$$

最终得到：

$$ \frac{\partial\rho_{e}}{\partial d_{j}^{i}}=\frac{1}{8}\sum_{i=1}^{8}H_{\varepsilon}^{\prime}\left(\phi_{e,i}^{s}\right)\cdot\left[\frac{\exp\left(\lambda\phi^{i}\right)}{\sum_{k=1}^{n}\exp\left(\lambda\phi^{k}\right)}\cdot\frac{\partial\phi^{i}}{\partial d_{j}^{i}}\right] \tag{31}$$

