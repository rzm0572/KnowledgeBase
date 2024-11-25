---
tags: 
  - Theory
---

# Numerical Differentiation and Integration

## Numerical Differentiation

### Use Lagrange Polynomial

- Forward-difference formula & Backward-difference formula:

$$
\newcommand{\dd}[1]{\mathrm{d}#1}
\newcommand{\intdd}[1]{\,\dd{#1}}
\begin{align*}
    f'(x) &= \frac{f(x + h) - f(x)}{h} - \frac{h}{2}f''(\xi), \\
    f'(x) &= \frac{f(x) - f(x - h)}{h} + \frac{h}{2}f''(\xi).
\end{align*}
$$

- (n+1)-Point formula:

$$
\begin{align*}
    f(x) &= \sum_{i=0}^n f(x_i) L_i(x) + \frac{\prod_{i=0}^n (x-x_i)}{(n+1)!} f^{(n+1)}(\xi), \\
    f'(x_j) &= \sum_{i=0}^n f(x_i) L_i'(x_j) + \frac{\prod_{i \neq j} (x_j - x_i)}{(n+1)!} f^{(n+1)}(\xi_j)
\end{align*}
$$

### Use Taylor Polynomial

$$
\begin{gather*}
    &{} \begin{cases}
        f(x+h) = f(x) + f'(x) h + \frac{1}{2} f''(x) h^2 + \frac{1}{3!} f'''(x) h^3 + \frac{1}{4!} f^{(4)}(\xi_1) h^4 \\
        f(x-h) = f(x) - f'(x) h + \frac{1}{2} f''(x) h^2 - \frac{1}{3!} f'''(x) h^3 + \frac{1}{4!} f^{(4)}(\xi_2) h^4
    \end{cases} \\
    \implies &{} f''(x) = \frac{1}{h^2} [f(x-h) - 2f(x) + f(x+h)] - \frac{1}{12} f^{(4)}(\xi) h^2
\end{gather*}
$$

### Richardson Extrapolation

假设我们有 $M$ 的如下估计式：

$$
    M = N_1(h) + \sum\limits_{i=1}^{\infty} K_{1i} h^i
$$

其中 $N_1(h)$ 后面的项为估计式 $N_1(h)$ 的截断误差 $R_1(h)$，可以看到 $R_1(h) \in O(h)$，下面我们使用 Richardson 外推法构造一个截断误差更小的估计式 $N_2(h)$：

$$
\begin{gather*}
    \begin{cases}
        M = N_1(h) + K_{11} h + K_{12} h^2 + \cdots \\
        M = N_1\left(\frac{h}{2}\right) + K_{11} \frac{h}{2} + K_{12} \frac{h^2}{4} + \cdots
    \end{cases} \\
    \implies M = 2N_1\left(\frac{h}{2}\right) - N_1(h) + \sum\limits_{i=2}^{\infty} \left( \frac{1}{2^{i-1}} - 1 \right) K_{1i} h^i
\end{gather*}
$$

令 $N_2(h) = 2N_1\left(\frac{h}{2}\right) - N_1(h)$，$K_{2i} = \left( \dfrac{1}{2^{i-1}} - 1 \right) K_{1i}$，则有

$$
    M = N_2(h) + K_{22} h^2 + K_{23} h^3 + \cdots
$$

由于 $|K_{2i}| \leq |K_{1i}|$，我们有 $R_2(h) \in O(h^2)$，以此类推，我们可以构造出估计式 $R_m(h)$，使得 $R_m(h) \in O(h^{2^{m-1}})$. 因此我们只需要几次外推就可以得到一个截断误差很小的估计式.

外推法特别适合用在使用 Taylor 展开来估计函数导数的情形.


## Numerical Integration (Numerical Quadrature)

目标：求 $I = \int_a^b f(x) \intdd{x}$ 满足给定精度的近似解.

!!! Definition "Definition :: Degree of accuracy / precision"
    称满足下列条件的最大正整数 $n$ 为数值积分方法的精确度（degree of accuracy / Precision）：

    设 $G(f, a, b)$ 为函数 $f(x)$ 在区间 $[a, b]$ 上的定积分 $\int_a^b f(x) \intdd{x}$ 的近似值，对于任意 $k = 0, 1, \cdots, n$，满足
    
    $$
        G(x^k, a, b) = \int_a^b x^k \intdd{x}
    $$

### Quadrature based on Lagrange Interpolation

基本思路：

- 在区间 $[a, b]$ 上取 $n+1$ 个点 $x_0, x_1, \cdots, x_n$，满足 $a = x_0 < x_1 < \cdots < x_n = b$.
- 以这 $n+1$ 个点为插值点，计算 Lagrange 基函数 $L_i(x)$.
- 将 Lagrange 插值多项式的积分作为 $f(x)$ 在 $[a, b]$ 上的定积分近似值，即

    $$
        \int_a^b f(x) \intdd{x} \approx \sum\limits_{i=0}^n f(x_i) L_i(x) \intdd{x}.
    $$

- 积分的误差为 Lagrange 插值的余项的积分，即

    $$
        R[f] = \int_a^b \frac{f^{(n+1)}(\xi_x)}{(n+1)!} \prod_{i=0}^n (x-x_i) \intdd{x}.
    $$

!!! Definition "Definition :: Newton-Cotes formulae"
    特别地，我们通常会在 $[a, b]$ 上等间距地取点，即 $x_i = a + i h$，其中 $h = \dfrac{b - a}{n}$，则有

    $$
        A_i = \int_a^b \prod_{j \neq i} \frac{(x - x_j)}{(x_i - x_j)} \intdd{x}
            = \int_0^n \prod_{j \neq i} \frac{(t-j)h}{(i-j)h} h \intdd{t}
            = \frac{(b-a) (-1)^{n-1}}{n! (n-i)!} \int_0^n \prod_{j \neq i} (t-j) \intdd{t}.
    $$

    式 $C_i^{(n)} = \dfrac{(-1)^{n-1}}{n! (n-i)!} \int_0^n \prod_{j \neq i} (t-j) \intdd{t}$ 被称为 Cotes 系数，只与 $n$ 和 $i$ 有关.

!!! Example "some common Newton-Cotes formulae"
    - Trapezoidal rule ($n=1$): $P = 1$
        
        $$
            \int_a^b f(x) \intdd{x} = \frac{b-a}{2} (f(a) + f(b)) - \frac{1}{12} h^3 f''(\xi), \enspace h = b-a.
        $$

    - Simpson rule ($n=2$): $P = 3$
        
        $$
            \int_a^b f(x) \intdd{x} = \frac{b-a}{6} (f(a) + 4f(\frac{a+b}{2}) + f(b)) - \frac{1}{90} h^5 f^{(4)}(\xi), \enspace h = \frac{b-a}{2}.
        $$
    
    - Simpson's 3/8 rule ($n=3$): $P = 3$

        $$
            R[f] = -\frac{3}{80} h^5 f^{(5)}(\xi), \enspace h = \frac{b-a}{3}.
        $$
    
    - Cotes Rule ($n=4$): $P = 5$

        $$
            R[f] = - \frac{8}{945} h^7 f^{(6)}(\xi), \enspace h = \frac{b-a}{4}.
        $$

!!! Theorem "Theorem :: Remainder of Newton-Cotes formulae"
    对于闭区间 $[a,b]$ 上 $n+1$ 阶的 Newton-Cotes 公式，存在 $\xi \in (a, b)$ 使得

    $$
        R[f] = \begin{cases}
            \dfrac{h^{n+3} f^{(n+2)}(\xi)}{(n+2)!} \int_0^n t^2 (t-1) \cdots (t-n) \intdd{t}, & n = 2k, \enspace f \in C^{n+2}[a, b]\\
            \dfrac{h^{n+2} f^{(n+1)}(\xi)}{(n+1)!} \int_0^n t (t-1) \cdots (t-n) \intdd{t}, & n = 2k+1, \enspace f \in C^{n+1}[a, b]
        \end{cases}
    $$

    其中 $h = \dfrac{b - a}{n}$.

### Composite Numerical Integration

在上节中，我们发现若使用 Newton-Cotes 公式，为了提高数值积分的精度就必须增加 Lagrange 插值多项式的阶数 $n$，然而对于一些函数，当 $n$ 较大时，其 Lagrange 插值多项式会出现振荡现象，导致误差增大. 下面我们将通过分段插值的方法解决这个问题，该方法被称作复合数值积分法.

基本思路：

- 将区间 $[a, b]$ 均分为 $n$ 个子区间 $[x_i, x_{i+1}]$
- 在每个子区间上使用 Newton-Cotes 公式计算积分近似值.
- 将结果累加得到最终结果.

复合数值积分法是稳定的，其采样点 $f(x_i)$ 的误差上界 $\varepsilon$ 和最后的积分值 $I$ 的误差上界 $\varepsilon_I$ 满足线性关系，即 $\varepsilon_I \leq (b-a) \varepsilon$.

!!! Example "Composite Trapezoidal rule"
    将区间 $[a, b]$ 均分为 $n$ 个子区间 $[x_i, x_{i+1}]$，其中 $x_k = a + k h, \enspace h = \dfrac{b - a}{n}$，则 $I$ 的估计式 $T_n$ 和余项 $R[f]$ 可表示为

    $$
    \begin{align*}
        T_n &= \sum\limits_{k=0}^{n-1} \frac{h}{2} (f(x_k) + f(x_{k+1})) = \frac{h}{2} \left( f(a) + 2 \sum\limits_{k=1}^{n-1} f(x_k) + f(b) \right), \\
        R[f] &= \sum\limits_{k=0}^{n-1} \left( -\frac{h^3}{12} f''(\xi_k) \right) = -\frac{h^2}{12} (b-a) f''(\xi).
    \end{align*}
    $$

!!! Example "Composite Simpson's rule"
    将区间 $[a, b]$ 均分为 $n$ 个子区间 $[x_i, x_{i+1}]$，其中 $x_k = a + k h, \enspace h = \dfrac{b - a}{n}$，则 $I$ 的估计式 $S_n$ 和余项 $R[f]$ 可表示为

    $$
    \begin{align*}
        S_n &= \sum\limits_{k=0}^{n-1} \frac{h/2}{3} \left( f(x_k) + 4f(x_{k + \frac{1}{2}}) + f(x_{k+1}) \right)
             = \frac{h}{6} \left( f(a) + f(b) + 2 \sum\limits_{k=1}^{n-1} f(x_k) + 4 \sum\limits_{k=0}^{n-1} f(x_{k + \frac{1}{2}}) \right), \\
        R[f] &= \sum\limits_{k=0}^{n-1} -\frac{(h/2)^5}{90} f^{(4)}(\xi_k)
              = -\frac{b-a}{180} \left( \frac{h}{2} \right)^4 f^{(4)}(\xi).
    \end{align*}
    $$

    令 $h' = \dfrac{h}{2}, \enspace x_k = a + k h'$，则有

    $$
        S_n = \frac{h'}{3} \left( f(a) + f(b) + 2 \sum\limits_{k=1}^{n-1} f(x_{2k}) + 4 \sum\limits_{k=0}^{n-1} f(x_{2k+1}) \right).
    $$

在实际应用中，我们常常取 $n=2^k$ 作为分段数.

### Romberg Integration

### Adaptive Quadrature Methods


