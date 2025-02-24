---
tags:
  - Theory
---

# Interpolation and Polynomial Approximation

## Polynomial Interpolation

!!! Definition "Definition :: Interpolation"
    给定一个函数 $f(x)$ 在多个不同的点 $x_i$ 处的函数值 $f(x_i)$，插值是指构造一个近似函数 $g(x)$，使得 $g(x_i) = f(x_i)$，并且 $g(x)$ 可以较好地逼近函数 $f(x)$.

多项式插值的理论基础：Weierstrass Approximation Theorem

即任意连续函数都可以使用一个多项式来近似.

## Lagrange Polynomials

!!! definition "Definition :: Lagrange Polynomials"
    对于函数 $f(x)$ 在其定义域上的 $n+1$ 个互异点 $x_0, x_1, \cdots, x_n$，存在次数不超过 $n$ 的多项式 $P(x)$，使得 $\forall k = 0, 1, \cdots, n$，

    $$
        f(x_k) = P(x_k).
    $$

    该多项式被称为 $n$ 阶 Lagrange 插值多项式.

!!! theorem "Theorem :: Lagrange 插值多项式的唯一性"
    按照上述方法定义的 Lagrange 插值多项式 $P(x)$ 是唯一的，其表达式为

    $$
        \newcommand{\dsum}{\displaystyle\sum}
        \newcommand{\dprod}{\displaystyle\prod}
        \newcommand{\dd}{\mathrm{d}}
        \newcommand{\intdd}{\,\mathrm{d}}
        \newcommand{\diff}[2]{\frac{\dd #1}{\dd #2}}
        P(x) = \dsum_{k=0}^n f(x_k) L_{n,k}(x),
    $$

    其中 $L_{n,k}(x)$ 是 $n$ 阶 Lagrange 插值基函数，满足

    $$
        L_{n,k}(x) = \dprod_{i \neq k} \frac{x-x_i}{x_k-x_i}.
    $$

!!! theorem "Theorem :: Lagrange 插值多项式的余项"
    若 $f \in C^{n+1}[a,b]$，插值点 $x_0, x_1, \cdots, x_n$ 互异且落在 $[a,b]$ 上，则对于任意 $x \in [a,b]$，存在 $\xi(x) \in (a,b)$，使得

    $$
        f(x) = P(x) + \frac{f^{(n+1)}(\xi(x))}{(n+1)!} \prod_{k=0}^n (x - x_k).
    $$

    Hint：类比 Taylor 展开的 Lagrange 余项来记忆.

??? example "Example :: 插值误差上界的估计"
    ![pE9IaSs.md.png](https://s21.ax1x.com/2025/01/07/pE9IaSs.md.png)

一般来说，

- 内插的误差比外插的误差小
- 选择更多的插值点时的误差较小，但是一方面由于次数增加会带来更多的舍入误差，另一方面还会出现荣格现象

我们发现从头计算 $n$ 阶 Lagrange 插值多项式是较麻烦的，因此我们考虑使用递推法进行计算，为了在已知低阶 Lagrange 插值多项式的情况下计算更高阶的 Lagrange 插值多项式，我们引入 Neville's Method：

!!! theorem "Theorem :: Neville's Method"
    对于任意函数 $f(x)$ 和插值点 $x_0, x_1, \cdots, x_n$，取 $\{x_i\}$ 的一个子集 $\{x_{m_i}\}, \enspace i = 1, 2, \cdots, k$，定义以 $x_{m_1}, x_{m_2}, \cdots, x_{m_k}$ 为插值点的 $k-1$ 阶 Lagrange 插值多项式为 $P_{m_1, m_2, \cdots, m_k}(x)$，则

    $$
        P_{0, 1, \ldots, k}(x) = \frac{(x - x_j) P_{0, 1, \ldots, j-1, j+1, \ldots, k}(x) - (x - x_i) P_{0, 1, \ldots, i-1, i+1, \ldots, k}(x)}{x_i - x_j}.
    $$

    于是我们可以参照以下顺序计算 $P_{0, 1, \ldots, n}(x)$：

    $$
    \begin{align*}
        x_0 & \rightarrow P_0 \searrow \\
        x_1 & \rightarrow P_1 \rightarrow P_{0,1} \searrow \\
        x_2 & \rightarrow P_2 \rightarrow P_{1,2} \rightarrow P_{0,1,2} \searrow \\
        x_3 & \rightarrow P_3 \rightarrow P_{2,3} \rightarrow P_{1,2,3} \rightarrow P_{0,1,2,3}
    \end{align*}
    $$

## Divided Differences


