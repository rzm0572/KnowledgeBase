---
tags: 
  - Theory
---

# Approximation Theory

## Discrete Least Squares Approximation

使用 $n$ 阶多项式 $P_n(x) = a_0 + a_1 x + a_2 x^2 + \cdots + a_n x^n$ 来拟合离散点列 $(x_i, y_i), i = 1, 2, \ldots, m$，其误差可表示为

$$
    E(a_0, a_1, \ldots, a_n) = \sum\limits_{i=1}^m (P_n(x_i) - y_i)^2.
$$

该式为一个 $\mathbb{R}^{n+1}$ 上关于 $a_0, a_1, \ldots, a_n$ 的二次型，故其拥有唯一极小值点，且同时为最小值点. 对 $a_0, a_1, \ldots, a_n$ 求导并令其为零，得到

$$
    \frac{\partial E}{\partial a_k} = 2 \sum\limits_{i=1}^m (P_n(x_i) - y_i) \frac{\partial P_n(x_i)}{\partial a_k} = 0.
$$

即

$$
    \sum\limits_{j=0}^n \left( a_j \sum\limits_{i=1}^m x_i^{j+k} \right) - \sum\limits_{i=1}^m y_i x_i^k = 0.
$$

令 $b_n = \sum\limits_{i=1}^m x_i^n$，$c_n = \sum\limits_{i=1}^m y_i x_i^n$，则我们只需要解以下线性系统

$$
    \begin{bmatrix}
        b_0 & b_1 & \cdots & b_n \\
        b_1 & b_2 & \cdots & b_{n+1} \\
        \vdots & \vdots & \ddots & \vdots \\
        b_n & b_{n+1} & \cdots & b_{2n}
    \end{bmatrix} \begin{bmatrix}
        a_0 \\ a_1 \\ \vdots \\ a_n
    \end{bmatrix} = \begin{bmatrix}
        c_0 \\ c_1 \\ \vdots \\ c_n
    \end{bmatrix}.
$$

可以证明，该线性系统有唯一解.

<!-- ??? proof
    TODO -->

!!! note
    当 $n = m-1$ 时，解出的多项式 $P_n(x)$ 即为 Lagrange 插值多项式.

## Orthogonal Polynomials and Least Squares Approximation

上一章中我们给出了离散点列的最小二乘拟合方法，本章中我们将给出连续函数的最小二乘拟合方法. 即对于任意 $[a, b]$ 上的连续函数 $f(x)$，我们希望找到一个 $n$ 阶多项式 $P_n(x)$ 以最小化

$$
    \newcommand{\dd}{\mathrm{d}}
    \newcommand{\dd}[1]{\mathrm{d}#1}
    \newcommand{\intdd}[1]{\,\dd{#1}}
    E(a_0, a_1, \ldots, a_n) = \int_a^b (P_n(x) - f(x))^2 \intdd{x}.
$$

我们希望可以找到一种方法可以简化上式的计算. 观察到 $P_n(x) \in \mathcal{P}_n(\mathbb{R})$，考虑找 $\mathcal{P}_n(\mathbb{R})$ 中与 $f(x)$ 最接近的多项式，这是一个最小化问题，可以使用正交投影法解决. 具体地，我们可以先找到 $P_n(x)$ 在 $[a, b]$ 上的正交基 $\{\phi_0(x), \phi_1(x), \ldots, \phi_n(x)\}$，$f(x)$ 在 $\mathcal{P}_n(\mathbb{R})$ 上的投影即为我们所需要的 $P_n(x)$.

多项式空间内的正交关系由内积的定义确定，我们可以取 $\langle f, g \rangle = \int_a^b w(x) f(x) g(x) \intdd{x}$ 作为内积的定义式，则有下列定义：

!!! definition "Definition :: Orthogonal set of Functions"
    函数组 $\{\phi_0, \phi_1, \ldots, \phi_n\}$ 被称作对函数 $\omega(x)$ 在 $[a, b]$ 上的正交函数集（Orthogonal set of functions），如果

    $$
        \int_a^b \phi_i(x) \phi_j(x) \intdd{x} = \begin{cases}
            0, & i \neq j \\
            m_i, & i = j
        \end{cases}.
    $$

    若 $m_i = 0, \enspace i = 0, 1, \ldots, n$，则称 $\{\phi_0, \phi_1, \ldots, \phi_n\}$ 为规范正交函数集（Orthonormal set of functions）.

线性代数中的 Gram-Schmidt 过程可以用来构造正交基，下面我们给出一种改进方法：

!!! note "正交基的构造方法"
    如果多项式组 $\{\varphi_0(x), \varphi_1(x), \ldots, \varphi_n(x)\}$ 满足以下递推关系：

    $$
    \begin{align*}
        \varphi_0 &= 1, \\
        \varphi_1 &= x - B_1, \\
        \varphi_k &= (x - B_k) \varphi_{k-1} - C_k \varphi_{k-2}.
    \end{align*}
    $$

    其中
    
    $$
        B_k = \frac{\langle x \varphi_{k-1}, \varphi_{k-1} \rangle}{\langle \varphi_{k-1}, \varphi_{k-1} \rangle}, \enspace
        C_k = \frac{\langle x \varphi_{k-1}, \varphi_{k-2} \rangle}{\langle \varphi_{k-2}, \varphi_{k-2} \rangle}.
    $$

    则 $\{\varphi_0(x), \varphi_1(x), \ldots, \varphi_n(x)\}$ 是 $\mathcal{P}_n(x)$ 的正交基.

    ??? example "几种特殊的正交多项式"

        === "Legendre 多项式"

            若内积的定义为 $\langle f, g \rangle = \int_{-1}^1 f(x) g(x) \intdd{x}$，则 $\{P_0(x), P_1(x), \ldots, P_n(x)\}$ 被称为勒让德多项式（Legendre polynomials）. 以下是其前若干项:

            $$
            \begin{gather*}
                P_0(x) = 1, \enspace
                P_1(x) = x, \enspace
                P_2(x) = x^2 - \frac{1}{3}, \\
                P_3(x) = x^3 - \frac{3}{5} x, \enspace
                P_4(x) = x^4 - \frac{6}{7} x^2 + \frac{3}{35}, \enspace
                P_5(x) = x^5 - \frac{10}{9} x^3 + \frac{5}{21} x.
            \end{gather*}
            $$

            性质：
            
            - $P_k(x) = \dfrac{1}{2^k k!} \dfrac{\dd^k}{\dd{x^k}} (x^2 - 1)^k$.
            - $\langle P_i, P_j \rangle = \dfrac{2}{2i + 1} \delta_{ij}$.
            - $(k+1)P_{k+1} = (2k+1)x P_k - kP_{k-1}$.

        === "Laguerre 多项式"

            若内积的定义为 $\langle f, g \rangle = \int_0^\infty f(x) g(x) e^{-x} \intdd{x}$，则 $\{L_0(x), L_1(x), \ldots, L_n(x)\}$ 被称为拉盖尔多项式（Laguerre polynomials）. 以下是其前若干项:

            $$
            \begin{gather*}
                L_0(x) = 1, \enspace
                L_1(x) = x - 1, \\
                L_2(x) = x^2 - 4x + 2, \enspace
                L_3(x) = x^3 - 9x^2 + 18x - 6
            \end{gather*}
            $$
        
        === "Chebyshev 多项式"

            若内积的定义为 $\langle f, g \rangle = \int_{-1}^1 \dfrac{1}{\sqrt{1 - x^2}} f(x) g(x) \intdd{x}$，则 $\{T_0(x), T_1(x), \ldots, T_n(x)\}$ 被称为切比雪夫多项式（Chebyshev polynomials）.

            $$
                T_k(x) = \cos(k \arccos(x)).
            $$

??? proof
    数学归纳法可证.




