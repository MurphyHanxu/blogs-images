

+++

author = "Murphy"
title = "Notes of Numerical Experiments"
date = "2023-03-01"

description = ""
tags = [
    "Math",
]

categories = [
    "CS",
   ]

math= true

toc=true

+++



# 数值实验

1.最小二乘问题

2.二维泊松方程的有限元方法

3.最速下降方法，CG方法，GMRES方法

4.非对称特征值问题

5.随机梯度下降方法



#### 1.最小二乘问题

最小二乘问题多产生于数据拟合问题。例如，假定给出$m$个点$t_1,\cdots,t_m$和这$m$个点上的实验或观测数据$y_1,\cdots,y_m$，并假定给出在$t_i$上取值的$n$个已知函数$\psi_1(t),\cdots,\psi_n(t)$。考虑$\psi_i$的线性组合。
$$
f(x,t)=x_1\psi_1(t)+x_2\psi_2(t)+\cdots+x_n\psi_n(t)
$$
我们希望在$t_1,\cdots,t_m$点上$f(x,t)$能最佳地逼近$y_1,\dots,y_m$这些数据。为此，若定义残量
$$
r_i(x)=y_i-\sum_{j=1}^nx_j\psi_j(t_i),\quad i=1,\cdots,m
\tag{1.1}
$$
则问题成为：估计参数$x_1,\cdots,x_n$，使残量$r_1,\cdots,r_m$尽可能地小。$(1.1)$式可用矩阵-向量形式表示为
$$
r(x)=b-Ax\tag{1.2}
$$
其中
$$
A=\begin{pmatrix}\psi_1(t_1)&\cdots&\psi_n(t_1)\\
\vdots& &\vdots\\
\psi_1(t_m) &\cdots&\psi_n(t_m)\end{pmatrix},
\quad
b=(y_1,\cdots,y_n)^T,\\
x=(x_1,\cdots,x_n)^T,\quad
r(x)=(r_1(x),\cdots,r_n(x))^T
$$
当$m=n$时，我们可以要求$r(x)=0$。当$m>n$时，一般不可能使所有残量为零，但我们可要求残向量$r(x)$在某种范数意义下最小。最小二乘问题就是求$x$使残向量$r(x)$在$2$范数意义下最小。



##### 定义1.1

给定矩阵$A\in \R^{m\times n}$及向量$b\in\R^m$，确定$x^{\ast}\in \R^{n}$使得
$$
\begin{align}x^{\ast}&=\text{argmin}_x\frac{1}{2}\Vert r(x)\Vert_2^2\\
&=\text{argmin}_x\frac{1}{2}\Vert b-Ax\Vert_2^2\\
&=\text{argmin}_x\frac{1}{2}\sum_{i=1}^m\Vert y_i-\sum_{j=1}^nx_j\psi_j(t_i)\Vert_2^2\\
\end{align}
$$
这就是所谓的最小二乘问题，简称LS（Least Squares）问题，其中的$r(x)$常常被称为残向量。

在所讨论的最小二乘问题中，若$r$线性地依赖于$x$，则称其为线性最小二乘问题。若$r$非线性地依赖于$x$，则称其为非线性最小二乘问题。

最小二乘问题的解$x^{\ast}$又可称做线性方程组
$$
Ax=b, \quad A\in \R^{m\times n}\tag{1.3}
$$
的最小二乘解，即$x^{\ast}$在残向量$r(x)=b-Ax$的$2$范数最小的意义下满足方程组$(1.3)$。

最小二乘问题可分为下面三种类型：

- $m=n$，$rank (A)=k\le m=n$。
- $m>n$，$rank(A)=k\le n=m$。称为超定方程组。
- $m<n$，$rank(A)=k\le m=n$。称为欠定方程组。



下面为了证明最小二乘问题解的存在性，首先定义一些记号。

设$A\in\R^{m\times n}$。$A$的值域定义为
$$
\text{Im}(A)=\{y\in \R^m|y=Ax,x\in \R^n\}
$$
易证$\text{Im}(A)=\text{span}(a_1,a_2,\cdots,a_n)$，其中$a_i(i=1,\cdots,n)$为$A$的列向量。

$A$的零空间定义为
$$
\ker(A)=\{x\in \R^n|Ax=0\}
$$
它的维数记为$\text{null}(A)$

一个子空间$S\subset\R^n$的正交补定义为：
$$
S^{\bot}=\{y\in\R^n|y^{T}x=0,\forall x\in S\}
$$


##### 定理1.1：最小二乘问题的解存在

下面从三个不同的视角证明最小二乘问题解的存在性。

视角1(数分视角)

设$f(x)=\frac{1}{2}\Vert b-Ax\Vert_2^2=\frac{1}{2}x^TA^TAx-x^TA^Tb+\frac{1}{2}\Vert b\Vert_2^2$

则$f'(x)=A^TAx-A^Tb$，当$A$列满秩时$A^TA$对称正定，$A^TA$可逆。

因此$x^{\ast}=(A^TA)^{-1}A^Tb$。

视角2(代数视角)：$\text{rank}(A)=\text{rank}([A,b])$

若$\text{rank}(A)=\text{rank}([A,b])$成立，则$b\in \text{Im}(A)$，即$b$可表示为$b=\sum_{i=1}^nx_ia_i$。这里$A=[a_1,a_2,\cdots,a_n]$。于是，令$x=(x_1,x_2,\cdots,x_n)^T$，即有$Ax=b$，从而解存在性得证。

视角3(泛函视角)

由题意即求$\text{min}\Vert b-Ax\Vert_2$一个泛函极小问题，在Hilbert空间上，$\text{min}\Vert b-Ax\Vert_2\ge \Vert b-Pb\Vert_2$，其中$P$为到$\text{Im}(A)$上的正交投影算子。而$Pb\in \text{A}$，则$\exist x^{\ast}$使得$Pb=Ax^{\ast}$，从而$P=A(A^TA)^{-1}A^T$，易于验证是幂等自伴的。



##### 定理1.2：假定方程组$(1.3)$的解存在，并且假定$x$是其任意给定的解，则方程组$(1.3)$的全部解的集合是

$$
x+\text{ker}(A)
$$

证明：如果$y$满足方程组$(1.3)$，则$A(y-x)=0$，即$y-x\in \text{ker}(A)$，于是有$y=x+(y-x)\in x+\text{ker}(A)$。反之，如果$y\in x+\text{ker}(A)$，则存在$z\in \text{A}$，使得$y=x+z$，从而有$Ay=Ax+Az=Ax=b$。



定理1.2告诉我们，只要知道了方程组$(1.3)$的一个解，便可以用它与$\text{ker}(A)$中的向量的和得到方程组$(1.3)$的全部解。由此可知，方程组$(3.14)$的解想要唯一，只有当$\text{ker}(A)$中仅有零向量才行。



##### 推论1.3：方程组$(1.3)$的解唯一的充分必要条件是

$$
\text{ker}(A)=\{0\}
$$

定理1.4：线性最小二乘问题$(1.3)$的解总是存在的，并且其解唯一的充分必要条件是$\text{ker}(A)=\{0\}$。

证明：因为$\R^m=\mathcal{R}(A)\oplus \mathcal{R}(A)^{\bot}$，所以向量$b$可以唯一地表示为$b=b_1+b_2$，其中$b_1\in\mathcal{R}(A)$，$b_2\in\mathcal{R}(A)^{\bot}$。于是，$\forall x \in \R^n$，$b_1-Ax\in\mathcal{R}(A)$且与$b_2$正交，从而
$$
\Vert r(x)\Vert_2^2=\Vert b-Ax\Vert_2^2=\Vert(b_1-Ax)+b_2\Vert_2^2=\Vert b_1-Ax\Vert_2^2+\Vert b_2\Vert^2_2
$$
由此可知，$\Vert r(x)\Vert_2^2$达到最小当且仅当$\Vert b_1-Ax\Vert_2^2$达到最小。而$b_1\in \mathcal{R}(A)$又蕴含着$\Vert b_1-Ax\Vert _2^2$达到最小的充分必要条件是$Ax=b_1$。这样，由$b_1\in\mathcal{R}(A)$和推论1.3立刻推出定理成立。



记最小二乘问题的解集为$\mathcal{X}_{LS}$，即
$$
\mathcal{X}_{LS}=\{x\in\R^n:x\text{是LS问题(1.3)的解}\}
$$
则由定理1.4只，$\mathcal{X}_{LS}$总是非空的，而且它仅有一个元素的充分必要条件是$A$的列线性无关。此外，不难证明解集中有且仅有一个解其$2$范数最小，我们用$x_{LS}$表示之，并称其为最小$2$范数解。



##### 定理1.5

$x\in \mathcal{X}_{LS}$当且仅当
$$
A^TAx=A^Tb\tag{1.4}
$$
证明：设$x\in \mathcal{X}_{LS}$。由定理$(1.4)$的证明知$Ax=b_1$，其中$b_1\in \mathcal{R}(A)$，而且
$$
r(x)=b-Ax=b-b_1=b_2\in\mathcal{R}(A)^{\bot}
$$
因此$A^Tr(x)=A^Tb_2=0$。将$r(x)=b-Ax$代入$A^Tr(x)=0$即得$(1.4)$式。

反之，设$x\in \R^n$满足$A^TAx=A^Tb$，则$\forall y\in \R^n$，有
$$
\begin{align}\Vert b-A(x+y)\Vert_2^2&=\Vert b-Ax\Vert_2^2-2y^TA^T(b-Ax)+\Vert Ay\Vert_2^2\\
&=\Vert b-Ax\Vert_2^2+\Vert Ay\Vert_2^2\\
&\ge\Vert b-Ax\Vert_2^2
\end{align}
$$
由此即得$x\in \mathcal{X}_{LS}$。



方程组$(1.4)$常常被称为最小二乘问题的正则化方程组或法方程组，它是一个含有$n$个变量和$n$个方程的线性方程组。在$A$的列向量线性无关的条件下，$A^TA$对称正定。
