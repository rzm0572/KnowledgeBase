---
tags: 
  - Theory
---

# Direct Methods for Solving Linear Systems


目标：解线性系统 $A \mathbf{x} = \mathbf{b}$，其中 $A$ 为 $n \times n$ 矩阵，$\mathbf{x}$ 为 $n$ 维向量，$\mathbf{b}$ 为 $n$ 维向量.

## Gaussian elimination

### Steps

!!! note "General Idea"
    先通过初等行变换将 $A$ 化为上三角矩阵，然后通过后向替代法（backward-substitution）求出每个未知数.

#### 化上三角矩阵

- 假设第 $k$ 步处理前的矩阵为 $A^{(k)}$，到达空间向量为 $\mathbf{b}^{(k)}$.
- 第 $k$ 步的主元（Pivot）：若 $a_{kk}^{(k)} \neq 0$，则选择 $a_{kk}^{(k)}$；若 $a_{kk}^{(k)} = 0$，则向下找到第一个非零元素 $a_{pk}^{(k)}$ 作为 Pivot，并交换第 $k$ 行与第 $p$ 行.
- (1.1) $\forall i = k+1, \ldots, n$，计算 $m_{ik} = \frac{a_{ik}^{(k)}}{a_{kk}^{(k)}}$
- (1.2) 计算 $A^{(k+1)}$ 与 $\mathbf{b}^{(k+1)}$，前 $k$ 行不变，后 $n-k$ 行满足

    $$
    \begin{cases}
        a_{ij}^{(k+1)} = a_{ij}^{(k)} - m_{ik} a_{kj}^{(k)} \\
        b_{i}^{(k+1)} = b_{i}^{(k)} - m_{ik} b_{k}^{(k)}
    \end{cases}
    $$

- 计算过程中若碰到 $a_{kk}^{(k)}$ 及以下元素全为 0 的情况，则说明方程组不可能有唯一解.

#### 后向替代

- (2.1) 首先，最后一行的元素 $x_{n}$ 可以直接得到：$x_{n} = \frac{b_{n}^{(n)}}{a_{nn}^{(n)}}$.
- (2.2) 假设到计算到第 $k$ 行时我们已经得到了 $x_{k+1}, \ldots, x_{n}$，则由于 $A^{(n)}$ 为上三角矩阵，我们有

    $$
        x_{k} = \frac{b_{k}^{(k)} - \sum_{i=k+1}^{n} a_{ki}^{(k)} x_{i}}{a_{kk}^{(k)}}
    $$

### Computation Times

考虑乘除法的次数：

- (1.1) 第 $k$ 步需要 $n-k$ 次乘除法，共 $\sum_{k=1}^n (n-k) = n(n-1)/2$ 次.
- (1.2) 第 $k$ 步需要 $(n-k)(n-k) + (n-k)$ 次乘除法，共 $\sum_{k=1}^n (n-k)(n-k+1) = (n^3 - n)/3$ 次.
- (2.1-2) 处理第 $k$ 行的未知数需要 $n-k+1$ 次乘除法，共 $\sum_{k=1}^n (n-k+1) = n(n+1)/2$ 次.

综上，高斯消元法共需要

$$
    \frac{n^3}{3} + n^2 - \frac{n}{3}
$$

次乘除法.

!!! info "Other Elimination Methods"
    除了 Gaussian elimination 外，还有其他的消元法，如 Gauss-Jordan elimination method：

    - 在使用初等行变换消元时，不仅将主元下方的元素消为 0，还将主元上方的元素也消为 0.
    - 共需要 $\frac{n^3}{2} + n^2 - \frac{n}{2}$ 次乘除法.

## Pivoting Strategies

### Why Pivoting?

- 化上三角矩阵过程中，若主元相对于同列的元素十分小，则 $m_{ik} = a_{ik}^{(k)} / a_{kk}^{(k)}$ 可能非常大，而 $m_{ik}$ 又会与 $A^{(k)}$ 的第 $k$ 行相乘并累加到第 $i$ 行，导致第 $k$ 行元素的舍入误差被放大.
- 后向替代过程中，主元 $a_{kk}^{(k)}$ 会作为分母，若 $a_{kk}^{(k)}$ 过小，则会放大分子的舍入误差.

### Partial Pivoting

- 在第 $k$ 次迭代中，我们需要找到最小的 $p \geqslant k$ 使得 $|a_{pk}^{(k)}| = \max\limits_{k \leqslant i \leqslant n} |a_{ik}^{(k)}|$，并将第 $k$ 行与第 $p$ 行交换.

- 增加的比较次数：$n(n-1)/2$

### Scaled Partial Pivoting

Partial Pivoting 只考虑了选取列方向上的最大元素，而没有将行方向上的元素考虑在内.

Scaled Partial Pivoting 在 Partial Pivoting 时将每行的元素除以该行绝对值最大元素的绝对值之后的结果作为判据，计算时还是使用原来的数据.

具体的，令

$$s_{i} = \max_{1 \leqslant j \leqslant n} |a_{ij}|,$$

在第 $k$ 次迭代中，我们需要找到最小的 $p \geqslant k$ 使得 $\dfrac{|a_{pk}^{(k)}|}{s_p} = \max\limits_{k \leqslant i \leqslant n} \dfrac{|a_{ik}^{(k)}|}{s_i}$，并将第 $k$ 行与第 $p$ 行交换.

注意：$s_{i}$ 只在开始的时候算一次，而非每次迭代完成后都重新计算.

- 增加的比较次数：$n(n-1) + \sum\limits_{k=2}^n k = \dfrac{3}{2}n(n-1)$.
- 增加的乘除法次数：$\sum\limits_{k=2}^n = n(n+1)/2-1$.

由于高斯消元法的乘除法次数为 $O(n^3)$，Scaled Partial Pivoting 不会带来显著的时间开销增长. 但如果每次迭代完成后都重新计算 $s_{i}$，则需要的比较次数增加到 $O(n^3)$.

### Complete Pivoting

Complete Pivoting (Maximal Pivoting) 通过选择全局最大元素来尽可能避免舍入误差的放大.

具体的，在第 $k$ 次迭代中，我们选择 $|a_{pq}^{(k)}| = \max\limits_{k \leqslant i,j \leqslant n} |a_{ij}^{(k)}|$ 作为主元，通过初等行变换与列变换将该元素交换到主元的位置.

- 增加的比较次数：$\sum\limits_{k=2}^n (k^2-1) = \dfrac{n(n-1)(2n-5)}{6}$.

当精度要求很高且时间要求不高时可以采用 Complete Pivoting.

## LU Factorization

在 $A$ 相同而 $\mathbf{b}$ 有多个的情况下，若使用 Gaussian Elimination 求解线性系统 $A \mathbf{x} = \mathbf{b}$，则每次求解都需要 $O(n^3)$ 次乘除法运算，我们需要用 LU 分解来减少计算量.

!!! Definition "Definition :: LU Factorization"
    设 $A$ 为 $n \times n$ 矩阵，$\mathbf{b}$ 为 $n$ 维向量，则 $A$ 的 LU 分解（LU factorization）是指存在两个 $n \times n$ 矩阵 $L$ 和 $U$，使得 $A = LU$，且 $L$ 为下三角矩阵，$U$ 为上三角矩阵，且 $L$ 的对角线元素均为 1.

!!! Theorem "Theorem :: Condition of LU Factorization"
    如果 $A$ 为 $n \times n$ 矩阵，且线性系统 $A \mathbf{x} = \mathbf{b}$ 可由 Gaussian Elimination 直接求解，则 $A$ 可进行 LU 分解.

<!-- !!! note "LU Factorization Steps" -->

考虑将高斯消元法的第 $k$ 步写成矩阵的形式，即使用左乘初等倍加矩阵的方式来表示行变换：

设 $m_{ik} = \dfrac{a_{ik}^{(k)}}{a_{kk}^{(k)}}$，则有初等倍加矩阵

$$
    L^{(k)} = \begin{bmatrix}
        1      & 0      &        &            & & & 0      \\
        0      & 1      &        &            & & &       \\
               &        & \ddots &            & & &       \\[0.9em]
               &        &        & 1          & & &       \\[1em]
               &        &        & -m_{k+1,k} & 1 & &       \\
               &        &        & \vdots     & & \ddots & 0      \\
        0      &        &        & -m_{nk}    & & & 1
    \end{bmatrix},
$$

满足

$$
    A^{(k+1)} = L^{(k)} A^{(k)},
$$

故有

$$
    A^{(n)} = L^{(n-1)} L^{(n-2)} \cdots L^{(1)} A.
$$

此时 $A^{(n)}$ 为上三角矩阵，设其为 $U$，并设

$$
    L = (L^{(1)})^{-1} (L^{(2)})^{-1} \cdots (L^{(n-1)})^{-1},
$$

即有

$$
    A = LU = \begin{bmatrix}
        1      & 0      & 0      & \cdots & 0      \\
        m_{21} & 1      & 0      & \cdots & 0      \\
        m_{31} & m_{32} & 1      & \cdots & 0      \\
        \vdots & \vdots & \vdots & \ddots & \vdots \\
        m_{n1} & m_{n2} & m_{n3} & \cdots & 1
    \end{bmatrix} \begin{bmatrix}
        a_{11}^{(1)} & a_{12}^{(1)} & a_{13}^{(1)} & \cdots & a_{1n}^{(1)} \\
        0            & a_{22}^{(2)} & a_{23}^{(2)} & \cdots & a_{2n}^{(2)} \\
        0            & 0            & a_{33}^{(3)} & \cdots & a_{3n}^{(3)} \\
        \vdots       & \vdots       & \vdots       & \ddots & \vdots       \\
        0            & 0            & 0            & \cdots & a_{nn}^{(n)}
    \end{bmatrix}.
$$

此时若我们要求解 $A \mathbf{x} = \mathbf{b}$，则只需完成一次前向替代和一次后向替代：

$$
    L \mathbf{y} = \mathbf{b}, \enspace U \mathbf{x} = \mathbf{y}.
$$

由于 $L$ 的对角线元素均为 1，为节省存储空间，常将 $L$ 与 $U$ 在相同的矩阵中存储，即

$$
    T = \begin{bmatrix}
        a_{11}^{(1)} & a_{12}^{(1)} & a_{13}^{(1)} & \cdots & a_{1n}^{(1)} \\
        m_{21}       & a_{22}^{(2)} & a_{23}^{(2)} & \cdots & a_{2n}^{(2)} \\
        m_{31}       & m_{32}       & a_{33}^{(3)} & \cdots & a_{3n}^{(3)} \\
        \vdots       & \vdots       & \vdots       & \ddots & \vdots       \\
        m_{n1}       & m_{n2}       & m_{n3}       & \cdots & a_{nn}^{(n)}
    \end{bmatrix}
$$

### Improvement

与 Gaussian Elimination 相似的，如果碰到 pivot 元素很小或是 0 的情况，则我们需要进行行交换操作，以保证结果的精度和正确性. 常用的方法是通过引入初等对换矩阵 $P$ 来实现 $A$ 的行交换，即将线性系统 $A \mathbf{x} = \mathbf{b}$ 改写为 $PA \mathbf{x} = P \mathbf{b}$，其中 $P$ 为 $n \times n$ 初等对换矩阵，然后再对 $PA$ 进行 LU 分解，此方法因此被称作 PLU 分解.

Tip:

- 此处引入的初等对换矩阵 $P$ 为正交矩阵，$P^T = P^{-1}$.
- $P$ 可在 Gaussian Elimination 过程中通过记录行交换的情况直接求得.

## Special Types of Matrices

### Strictly Diagonally Dominant Matrices
