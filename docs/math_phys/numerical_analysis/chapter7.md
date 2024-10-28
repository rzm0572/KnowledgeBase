---
tags: 
  - Theory
---


# Iterative Techniques in Matrix Algebra

## Norms

### Vector Norms

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
    矩阵的谱半径 (spectral radius) 是指矩阵的特征值中绝对值最大的那个值，记作 $\rho(A)$，即 $\rho(A) = \max\limits_{\lambda \in \Lambda(A)} |\lambda|$，其中 $\Lambda(A)$ 是矩阵 $A$ 的特征值的集合.

对于任意自然范数 $\Vert \cdot \Vert_p$，都有 $\rho(A) \leq \Vert A \Vert_p$，即矩阵的谱半径小于等于矩阵的范数.

!!! Definition "Definiton :: Convergence"
    称 $n \times n$ 矩阵 $A$ 收敛，如果对于任意 $i, j = 1, 2, \ldots , n$，都有
    
    $$
        \lim_{k \to \infty} \left( A^k \right)_{ij} = 0.
    $$

矩阵 $A$ 收敛的一个充分条件：如果 $A$ 可以分解为 $A = P L P^{-1}$，其中 $P$ 是 $n \times n$ 正交矩阵，$L$ 是 $n \times n$ 对角矩阵，且对角元素为 $\lambda_1, \lambda_2, \ldots, \lambda_n$，则当 $|\lambda_i| < 1$ 时，$A$ 收敛.

!!! theorem "Theorem :: Equivalent conditions for convergence"
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

令 $T_j = D^{-1} (L + U), \mathbf{c}_j = D^{-1} \mathbf{b}$，则得到迭代式 $\mathbf{x}_{i + 1} = T_j \mathbf{x}_i + \mathbf{c}_j$.

对于 $a_{ii} = 0$ 的情况，我们可以通过交换 $A$ 的行的方式使得 $a_{ii}$ 不为零，然后再进行迭代. 若无法通过行交换使得 $a_{ii} \neq 0$，说明 $A$ 不是可逆矩阵，原方程没有唯一解.

### Gauss-Seidel Iterative Method

Gauss-Seidel 迭代法是 Jacobi 迭代法的改进，其迭代式为

$$
    \mathbf{x}_k = D^{-1} (\mathbf{b} + L \mathbf{x}_k + U \mathbf{x}_{k-1}).
$$

即

$$
    \mathbf{x}_k = (D - L)^{-1} U \mathbf{x}_{k-1} + (D - L)^{-1} \mathbf{b}.
$$

令 $T_g = (D - L)^{-1} U, \mathbf{c}_g = (D - L)^{-1} \mathbf{b}$，则得到迭代式 $\mathbf{x}_{i+1} = T_g \mathbf{x}_k + \mathbf{c}_g$.

!!! advice "Pros"
    - $\mathbf{x}$ 中的每个元素在被计算后就会直接在下一个元素的计算中被使用，使得更多的信息被利用.
    - Jacobi 迭代法的收敛速度受初值的影响较大，而 Gauss-Seidel 迭代法可以自动确定一个比较好的初值，使得收敛速度更快.

!!! not-advice "Cons"
    - 当矩阵不是对角占优的时候可能会不收敛.

