---
tags: 
  - Theory
---

# Iterative Techniques in Matrix Algebra

## Norms

### Vector Norms

!!! Definition "Definition :: Vector Norm"
    一个从 $\mathbb{R}^n$ 到 $\mathbb{R}$ 的映射 $\Vert \cdot \Vert$ 被称作向量范数，如果其对任意的 $\mathbf{\alpha}, \mathbf{\beta} \in \mathbb{R}^n$ 满足：

    1. 正定性：$\Vert \mathbf{\alpha} \Vert \geq 0$，且 $\Vert \mathbf{\alpha} \Vert = 0 \iff \mathbf{\alpha} = \mathbf{0}$.
    2. 齐次性：$\forall \lambda \in \mathbb{C}, \enspace \Vert \lambda \mathbf{\alpha} \Vert = \vert \lambda \vert \Vert \mathbf{\alpha} \Vert$.
    3. 三角不等式：$\Vert \mathbf{\alpha} + \mathbf{\beta} \Vert \leq \Vert \mathbf{\alpha} \Vert + \Vert \mathbf{\beta} \Vert$.

引入向量范数是为了表征向量之间的“距离”，从而我们可以定义向量序列的收敛：

!!! Definition "Definition :: Convergence of Vector"
    称向量序列 $\{\mathbf{x}_n\}_{n=1}^\infty$ 在范数 $\Vert \cdot \Vert$ 下收敛到 $\mathbf{x}$，如果对任意的 $\varepsilon > 0$，都存在 $N(\varepsilon)$ 使得对所有 $n > N(\varepsilon)$，有 $\Vert \mathbf{x}_n - \mathbf{x} \Vert < \varepsilon$，即 $\lim\limits_{n\to\infty} \Vert \mathbf{x}_n - \mathbf{x} \Vert = 0$.

所有向量范数都是等价的，即对于任意两种向量范数 $\Vert \cdot \Vert_1$ 和 $\Vert \cdot \Vert_2$，都存在 $C_1, C_2$ 使得 $\forall \mathbf{x} \in \mathbb{R}^n$，有 $C_1 \Vert \mathbf{x} \Vert_1 \leq \Vert \mathbf{x} \Vert_2 \leq C_2 \Vert \mathbf{x} \Vert_1$.

常用的向量范数有：

1. $L_1$ 范数：$\Vert \mathbf{x} \Vert_1 = \sum\limits_{i=1}^n |x_i|$.
2. $L_2$ 范数：$\Vert \mathbf{x} \Vert_2 = \sqrt{\sum\limits_{i=1}^n x_i^2}$.
3. $L_\infty$ 范数：$\Vert \mathbf{x} \Vert_\infty = \max\limits_{1 \leqslant i \leqslant n} |x_i|$.

### Matrix Norms

!!! Definition "Definiton :: Matrix Norm"
    一个从 $\mathbb{R}^{n\times n}$ 到 $\mathbb{R}$ 的映射 $\Vert \cdot \Vert$ 被称作矩阵范数，如果其对任意的 $A, B \in \mathbb{R}^{n\times n}$ 满足：

    1. 正定性：$\Vert A \Vert \geq 0$，且 $\Vert A \Vert = 0 \iff A = 0$.
    2. 齐次性：$\forall \alpha \in \mathbb{C}, \enspace \Vert \alpha A \Vert = \vert \alpha \vert \Vert A \Vert$.
    3. 三角不等式：$\Vert A + B \Vert \leq \Vert A \Vert + \Vert B \Vert$.
    4. 一致性：$\Vert AB \Vert \leq \Vert A \Vert \Vert B \Vert$.

引入一致性是为了在分析迭代法收敛性时分析矩阵相乘带来的误差上界.

下面列举一些常用的矩阵范数：

1. Frobenius Norm:

    - $\Vert A \Vert_F = \sqrt{\sum\limits_{i=1}^n \sum\limits_{j=1}^n |a_{ij}|^2}$.

    - Frobenius Norm 相当于把矩阵看作一个 $n \times n$ 的向量时的 L2 范数.

2. Natural Norm:

    - $\Vert A \Vert_p = \max\limits_{\Vert x \Vert_p = 1} \Vert A \mathbf{x} \Vert_p = \max\limits_{\mathbf{x} \in \mathbb{R}^n} \left\Vert \dfrac{A\mathbf{x}}{\Vert \mathbf{x} \Vert_p} \right\Vert_p$.
    - $\Vert A \Vert_p \Vert \mathbf{x} \Vert_p \geqslant \Vert A \mathbf{x} \Vert_p$.
    - 描述了矩阵作用在向量上的效果
!!! example "几种特殊情况"

    无穷范数：$\Vert A \Vert_{\infty} = \max\limits_{1 \leqslant i \leqslant n} \sum\limits_{j=1}^n |a_{ij}|$.

    L1 范数：$\Vert A \Vert_1 = \max\limits_{1 \leqslant j \leqslant n} \sum\limits_{i=1}^n |a_{ij}|$.

    L2 范数（谱范数）：$\Vert A \Vert_{2} = \sqrt{\lambda_{\max}(A^T A)}$.

## Eigenvalues and Eigenvectors

!!! Definition "Difinition :: Spectral Radius"
    矩阵的谱半径 (spectral radius) 是指矩阵的特征值中模长最大的那个值，记作 $\rho(A)$，即 $\rho(A) = \max\limits_{\lambda \in \Lambda(A)} |\lambda|$，其中 $\Lambda(A)$ 是矩阵 $A$ 的特征值的集合.

对于任意自然范数 $\Vert \cdot \Vert_p$，都有 $\rho(A) \leq \Vert A \Vert_p$，即矩阵的谱半径小于等于矩阵的自然范数.

!!! Definition "Definiton :: Convergence of Matrix"
    称 $n \times n$ 矩阵 $A$ 收敛，如果对于任意 $i, j = 1, 2, \ldots , n$，都有

    $$
        \lim_{k \to \infty} \left( A^k \right)_{ij} = 0.
    $$

矩阵 $A$ 收敛的一个充分条件：如果 $A$ 可以分解为 $A = P L P^{-1}$，其中 $P$ 是 $n \times n$ 正交矩阵，$L$ 是 $n \times n$ 对角矩阵，且对角元素为 $\lambda_1, \lambda_2, \ldots, \lambda_n$，则当 $|\lambda_i| < 1$ 时，$A$ 收敛.

!!! theorem "Theorem :: Equivalent conditions for convergence of matrix"
    下列四个命题是等价的：

    1. $A$ 收敛.
    2. 对于某个自然范数 $\Vert \cdot \Vert_p$， $\lim\limits_{n \to \infty} \Vert A^n \Vert_p = 0$.
    3. 对于任意自然范数 $\Vert \cdot \Vert_p$，$\lim\limits_{n \to \infty} \Vert A^n \Vert_p = 0$.
    4. $\rho(A) < 1$.
    5. 对于任意 $\mathbf{x} \in \mathbb{R}^n$，$\lim\limits_{n \to \infty} A^n \mathbf{x} = \mathbf{0}$.

## Iterative Techniques for Solving Linear Systems

### Jacobi Iterative Method

设 $A\mathbf{x} = \mathbf{b}$ 为待求解的线性系统，$A = (a_{ij})_{n \times n}$. 我们设 $L$ 为 $A$ 只保留下三角部分的矩阵，$U$ 为 $A$ 只保留上三角部分的矩阵，$D$ 为 $A$ 只保留对角线元素的矩阵，即

$$
    \begin{gather}
        L = \begin{bmatrix}
            0 & 0 & 0 & \cdots & 0 \\
            -a_{21} & 0 & 0 & \cdots & 0 \\
            -a_{31} & -a_{32} & 0 & \cdots & 0 \\
            \vdots & \vdots & \vdots & \ddots & \vdots \\
            -a_{n1} & -a_{n2} & -a_{n3} & \cdots & 0
        \end{bmatrix}, \quad
        U = \begin{bmatrix}
            0 & -a_{12} & -a_{13} & \cdots & -a_{1n} \\
            0 & 0 & -a_{23} & \cdots & -a_{2n} \\
            0 & 0 & 0 & \cdots & -a_{3n} \\
            \vdots & \vdots & \vdots & \ddots & \vdots \\
            0 & 0 & 0 & \cdots & 0
        \end{bmatrix}, \\[2em]
        D = \mathrm{diag}(a_{11}, a_{22}, \ldots, a_{nn}).
    \end{gather}
$$

则

$$
    A \mathbf{x} = \mathbf{b} \iff \mathbf{x} = D^{-1} (L + U) \mathbf{x} + D^{-1} \mathbf{b}.
$$

令 $T_j = D^{-1} (L + U), \mathbf{c}_j = D^{-1} \mathbf{b}$，则得到迭代式 $\mathbf{x}^{(i + 1)} = T_j \mathbf{x}^{(i)} + \mathbf{c}_j$.

对于 $a_{ii} = 0$ 的情况，我们可以通过交换 $A$ 的行的方式使得 $a_{ii}$ 不为零，然后再进行迭代. 若无法通过行交换使得 $a_{ii} \neq 0$，说明 $A$ 不是可逆矩阵，原方程没有唯一解.

### Gauss-Seidel Iterative Method

Gauss-Seidel 迭代法是 Jacobi 迭代法的改进，其迭代式为

$$
    \mathbf{x}^{(k)} = D^{-1} (\mathbf{b} + L \mathbf{x}^{(k)} + U \mathbf{x}^{(k-1)}).
$$

即

$$
    \mathbf{x}^{(k)} = (D - L)^{-1} U \mathbf{x}^{(k-1)} + (D - L)^{-1} \mathbf{b}.
$$

令 $T_g = (D - L)^{-1} U, \mathbf{c}_g = (D - L)^{-1} \mathbf{b}$，则得到迭代式 $\mathbf{x}_{i+1} = T_g \mathbf{x}^{(k)} + \mathbf{c}_g$.

!!! advice "Pros"
    - $\mathbf{x}$ 中的每个元素在被计算后就会直接在下一个元素的计算中被使用，使得更多的信息被利用.
    - Jacobi 迭代法的收敛速度受初值的影响较大，而 Gauss-Seidel 迭代法可以自动确定一个比较好的初值，使得收敛速度更快.
    - 可以在原本的向量上进行迭代，而不需要额外的空间.

### Convergence Analysis of two methods

#### Convergence Condition

假设迭代式为 $\mathbf{x}^{(k)} = T \mathbf{x}^{(k-1)} + \mathbf{c}$，将其不断迭代，有

$$
    \mathbf{x}^{(k)} = T \mathbf{x}^{(k-1)} + \mathbf{c}
                 = T (T \mathbf{x}^{(k-2)} + \mathbf{c}) + \mathbf{c}
                 = \cdots
                 = T^k \mathbf{x}^{(0)} + \sum_{i=0}^{k-1} T^i \mathbf{c}.
$$

当 $k \to +\infty$ 时，$\sum\limits_{i=0}^{k-1} T^i \mathbf{c} = \sum\limits_{i=0}^{\infty} T^i \mathbf{c} = (I - T)^{-1} \mathbf{c}$. 故只要 $I-T$ 可逆，且 $T^k \mathbf{x}^{(0)}$ 收敛，就有 $\mathbf{x}^{(k)}$ 收敛.

!!! Theorem "Theorem :: 向量不动点定理"
    若向量序列 $\{\mathbf{x}^{(n)}\}_{n=1}^\infty$ 的递推式为 $\mathbf{x}^{(k)} = T \mathbf{x}^{(k-1)} + \mathbf{c}$，则该向量序列收敛到 $\mathbf{x} = T \mathbf{x} + \mathbf{c}$ 的唯一解当且仅当 $\rho(T) < 1$.

??? Proof
    由矩阵收敛的等价条件可以证明.

#### Error Estimation

在上述向量序列收敛的情况下，我们可以给出其误差的上界：

设 $\mathbf{x}^*$ 为 $\mathbf{x}^{(k)} = T \mathbf{x}^{(k-1)} + \mathbf{c}$ 的唯一不动点，则

$$
    \begin{align*}
        \Vert \mathbf{x}^{(k)} - \mathbf{x}^* \Vert &\leqslant \Vert T \Vert^k \Vert \mathbf{x}^{(0)} - \mathbf{x}^* \Vert \\
        \Vert \mathbf{x}^{(k)} - \mathbf{x}^* \Vert &\leqslant \frac{\Vert T \Vert^k}{1 - \Vert T \Vert} \Vert \mathbf{x}_1 - \mathbf{x}^* \Vert
    \end{align*}
$$

另外，误差 $\Vert \mathbf{x}^{(k)} - \mathbf{x}^* \Vert$ 也存在估计公式：

$$
    \Vert \mathbf{x}^{(k)} - \mathbf{x}^* \Vert \approx \rho(T)^k \Vert \mathbf{x}^{(0)} - \mathbf{x}^* \Vert.
$$

#### Relaxation Method

在 Gauss-Seidel 迭代法中，设 $r_{mi}^{(k)}$ 表示在第 $k$ 次迭代时，$x_i$ 更新前第 $m$ 个方程的残差（Residual），即

$$
    r_{mi}^{(k)} = b_i - \sum\limits_{j=1}^{i-1} a_{mj} x_j^{(k)} - \sum\limits_{j=i}^{n} a_{mj} x_j^{(k-1)}.
$$

可以发现，Gauss-Seidel 迭代法的迭代式还可以使用残差的形式写成

$$
    x_{i}^{(k)} = \frac{1}{a_{ii}} \left(b_i - \sum\limits_{j=1}^{i-1} a_{ij} x_j^{(k)} - \sum\limits_{j=i+1}^{n} a_{ij} x_j^{(k-1)}\right) = x_{i}^{(k-1)} + \frac{r_{ii}^{(k)}}{a_{ii}}.
$$

此时，如果我们在该递推式中添加一个系数 $\omega$，将其变为 $x_{i}^{(k)} = x_{i}^{(k-1)} + \omega \dfrac{r_{ii}^{(k)}}{a_{ii}}$，则可以调整收敛速度. 这种方法叫做 Relaxation Method.

- $0 < \omega < 1$ 时，称为 Under-relaxation Method.
- $\omega = 1$ 时，即为 Gauss-Seidel Method.
- $\omega > 1$ 时，称为 Successive Over-relaxation Method (SOC).

SOC 方法可以在某些条件下加速线性系统的收敛速度，写成矩阵形式为

$$
    \mathbf{x}^{(k)} = T_{\omega} \mathbf{x}^{(k-1)} + \mathbf{c}_{\omega},
$$

其中 $T_{\omega} = (D - \omega L)^{-1} [(1 - \omega) D + \omega U], \enspace \mathbf{c}_{\omega} = \omega (D - \omega L)^{-1} \mathbf{b}$.

SOC 方法收敛的条件如下：

!!! Theorem "Theorem :: Kahan（SOC 方法收敛的必要条件）"
    对于对角线不为零的矩阵 $A$ 组成的线性系统 $A\mathbf{x} = \mathbf{b}$，$\rho(T_{\omega}) \geqslant | \omega - 1 |$.

    推论：SOC 方法能够收敛 $\implies \omega \in (0, 2)$.

!!! Theorem "Theorem :: Ostowski-Reich（SOC 方法收敛的一个充分条件）"
    如果 $A$ 是正定的，并且 $\omega \in (0, 2)$，则不论初值 $\mathbf{x}^{(0)}$ 取任何值，SOC 方法均可收敛.

!!! Theorem
    如果 $A$ 是正定的三对角矩阵（tridiagonal matrix），则 $\rho(T_g) = [\rho(T_j)]^2 < 1$，并且 SOC 方法中

    $$
        \omega_0 = \frac{2}{1 + \sqrt{1 - \rho(T_j)^2}}
    $$

    为 $\omega$ 的最优值，此时 $\rho(T_{\omega_0}) = \omega_0 - 1$.

## Error Bounds and Iterative Refinement

### Error Bounds

当 $A$ 存在误差 $\delta A$，$\mathbf{b}$ 存在误差 $\delta \mathbf{b}$ 时，我们需要给出解 $\mathbf{x}$ 的误差 $\delta \mathbf{x}$ 的估计. 我们还想知道在什么条件下 $A$ 和 $\mathbf{b}$ 的误差会对解的误差产生显著影响.

我们设 Condition number 为 $K(A) = \Vert A \Vert \Vert A^{-1} \Vert$.

先考虑 $A$ 是精确的，$\mathbf{b}$ 存在误差 $\delta \mathbf{b}$ 的情况，即满足下列方程：

$$
\begin{cases}
    A (\mathbf{x} + \delta \mathbf{x}) = \mathbf{b} + \delta \mathbf{b} \\
    A \mathbf{x} = \mathbf{b}
\end{cases}
$$

该情况下，我们可以得到 $\delta \mathbf{x}$ 的估计：

$$
    \frac{\Vert \delta \mathbf{x} \Vert}{\Vert \mathbf{x} \Vert} \leqslant \Vert A \Vert \Vert A^{-1} \Vert \frac{\Vert \delta \mathbf{b} \Vert}{\Vert \mathbf{b} \Vert} = K(A) \frac{\Vert \delta \mathbf{b} \Vert}{\Vert \mathbf{b} \Vert}.
$$

??? Proof

再考虑 $b$ 是精确的，而 $A$ 存在误差 $\delta A$ 的情况，即满足下列方程：

$$
\begin{cases}
    (A + \delta A)(\mathbf{x} + \delta \mathbf{x}) = \mathbf{b} \\
    A \mathbf{x} = \mathbf{b}
\end{cases}
$$

该情况下，我们可以得到 $\delta \mathbf{x}$ 的估计：

$$
    \frac{\Vert \delta \mathbf{x} \Vert}{\Vert \mathbf{x} \Vert} \leqslant \frac{\Vert A^{-1} \Vert \Vert \delta A \Vert}{1 - \Vert A^{-1} \Vert \Vert \delta A \Vert} = \frac{K(A) \frac{\Vert \delta A \Vert}{\Vert A \Vert}}{1 - K(A) \frac{\Vert \delta A \Vert}{\Vert A \Vert}}.
$$

??? Proof

综上，将两者合并，我们得到在 $A$ y $\mathbf{b}$ 存在误差的情况下，$\delta \mathbf{x}$ 的估计：

$$
    \frac{\Vert \delta \mathbf{x} \Vert}{\Vert \mathbf{x} \Vert} \leqslant \frac{K(A)}{1 - K(A) \frac{\Vert \delta A \Vert}{\Vert A \Vert}} \left(\frac{\Vert \delta A \Vert}{\Vert A \Vert} + \frac{\Vert \delta \mathbf{b} \Vert}{\Vert \mathbf{b} \Vert}\right).
$$

可以看到，解的误差除了受 $A$ 和 $\mathbf{b}$ 的误差大小影响外，还受到 $K(A)$ 的影响.

- 当 $K(A) \approx 1$ 时，我们称之为 well-conditioned，此时 $A$ 和 $\mathbf{b}$ 的误差对解的误差影响较小；
- 当 $K(A) \gg 1$ 时，我们称之为 ill-conditioned，此时 $A$ 和 $\mathbf{b}$ 的误差会被显著放大，对解的误差影响较大.

!!! property "Property :: Condition Number"
    $K(A)$ 有以下几条性质：

    - 如果 $A$ 是对称矩阵，则 $K(A)_2 = \dfrac{\max |\lambda|}{\min |\lambda|}$.
    - 对于任意自然范数 $\Vert \cdot \Vert$，$K(A)_p \geqslant 1$.
    - $\forall \alpha \in \mathbb{R}, \enspace K(\alpha A) = K(A)$.
    - $K(A)_2 = 1 \iff A$ 是正定的.
    - 对于任意正定矩阵 $R$, $K(RA)_2 = K(AR)_2 = K(A)_2$.

### Iterative Refinement | 迭代细化
