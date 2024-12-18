---
tags: 
  - Theory
---

# Solution of Equations in One Variable

## Bisection Method | 二分法

**Attention:**

- Set $p = a + \frac{b-a}{2}$ in iterations.

    原因：需要考虑舍入误差。e.g. 取三位有效数字，$a=0.563, b=0.564$，则 $p=\frac{a+b}{2}=\frac{1.13}{2}=0.565 \notin [a,b]$

- Use $\text{sgn} (f(p))$ to determine the side of the root.

## Fixed-Point Iteration | 不动点法

### Why it works

下面给出不动点法求一元方程数值解的理论基础.

!!! Theorem "Theorem :: Fixed-Point Theorem | 不动点定理"
  
    若函数 $g(x)$ 满足以下条件：
    
    - $g(x)$ 在闭区间 $[a,b]$ 上连续；

    - $\forall x \in [a,b], \enspace g(x) \in [a,b]$；

    - $g(x)$ 在开区间 $(a,b)$ 上可导；

    - $\forall x \in (a,b), \exists k \in (0,1), \enspace |g'(x)| < k$.

    则对于满足迭代关系 $p_n = g(p_{n-1})$ 的数列 $\{p_n\}$，若其初值 $p_0 \in (a,b)$，则 $\{p_n\}$ 收敛到 $g(x)$ 在 $(a,b)$ 上的不动点 $p$，且 $p$ 是唯一的.

!!! Proof

    证明：

不动点定理保证了按照满足条件的函数 $g(x)$ 进行迭代后数列能够收敛到唯一的不动点，即所求的根.

### How it works

## Evaluation of Iterative Methods

我们使用收敛阶数来衡量迭代方式的收敛速度：

!!! Definition "Definition :: Convergence Order | 收敛阶数"
    设迭代数列 $\{p_n\}$ 收敛到 $p$，且 $\forall i \in \mathbf{N}, \enspace p_i \neq p$. 若存在正常数 $\alpha$ 与 $\lambda$，使得

    $$
        \lim\limits_{n \to \infty} \frac{|p_{n+1} - p|}{|p_n - p|^\alpha} = \lambda,
    $$
    
    则称 $\{p_n\}$ 的收敛阶数（Convergence Order）为 $\alpha$，渐进误差常数（Asymptotic Error Constant）为 $\lambda$.

特别地，

- 若 $\alpha = 1$，则称 $\{p_n\}$ 线性收敛（Linear Convergent）

- 若 $\alpha = 2$，则称 $\{p_n\}$ 二次收敛（Quadratic Convergent）

二次收敛的收敛速度远远快于线性收敛.

!!! Theorem
    对于迭代关系为 $p_{n+1} = g(p_n)$ 的数列，其不动点为 $p$，若存在常数 $\alpha \geqslant 2$ 与 $\delta > 0$，使得

    1. $g(x)$ 在 $U(p, \delta)$ 上 $\alpha$ 阶连续
    
    2. $g^{(\alpha)}(x)$ 在 $U(p, \delta)$ 上有界

    3. $g'(p) = g''(p) = \cdots = g^{(\alpha-1)}(p) = 0, \enspace g^{(\alpha)}(p) \neq 0$

    4. 数列初值 $p_0 \in U(p, \delta)$


    则 $\{p_n\}$ 的收敛阶数为 $\alpha$.

!!! Proof
    使用泰勒公式可证.

## Newton-Raphson Method | 牛顿迭代法

$$
    p_n = p_{n-1} - \frac{f(p_{n-1})}{f'(p_{n-1})}
$$

<!-- <img src="/KnowledgeBase/assets/NA/1.png" style="width: 60%;"> -->
<!-- [![pAN4iq0.md.png](https://s21.ax1x.com/2024/10/16/pAN4iq0.md.png)](https://imgse.com/i/pAN4iq0) -->

## 优化牛顿迭代法

$m$ 重根定义：$f(x) = 0$ 的一个根 $p$ 被称为 $f(x)$ 的 $m$ 重根，如果 $\forall x \neq p, \enspace f(x) = (x-p)^m q(p)$，其中 $\lim\limits_{x \to p} q(x) = 0$.

对于重根情况，牛顿迭代法退化为线性收敛.

考虑构造

$$
    \mu(x) = \frac{f(x)}{f'(x)}
$$

然后对 $\mu(x)$ 进行牛顿迭代，即

$$
    g(x) = x - \frac{\mu(x)}{\mu'(x)}
         = x - \frac{f(x)f'(x)}{[f'(x)]^2 -f(x)f''(x)}
$$

该迭代式能实现二次收敛，原因是将 $f(x) = (x-p)^m q(p)$ 代入 $\mu(x)$ 后可得

$$
    \mu(x) = (x-p) \frac{q(x)}{mq(x) + (x-p)q'(x)}
$$

有单根 $p$.

!!! not-advice "Cons"
    1. 需要计算 $f''(x)$；
    2. 迭代公式中的分母中的两项 $[f'(x)]^2$ 与 $f(x)f''(x)$ 均接近 0，会造成较大的舍入误差.

## Aitken's $\Delta^2$ Method

$$
    \hat{p_n} = p_n - \frac{(p_{n+1} - p_n)^2}{p_n - 2p_{n+1} + p_{n+2}}.
$$

差分写法：

$$
    \hat{p_n} = \{\Delta^2\}(p_n) = p_n - \frac{(\Delta p_n)^2}{\Delta^2 p_n}
$$

<div class="grid" markdown>

[![pAdmZQK.md.png](https://s21.ax1x.com/2024/10/22/pAdmZQK.md.png)](https://imgse.com/i/pAdmZQK)

!!! Info "Steps"
    1. 取 $p_0$ 作为初值；
    2. 计算 $p_1 = g(p_0), p_2 = g(p_1)$；
    3. 计算 $\hat{p_0} = p_0 - \frac{(\Delta p_0)^2}{\Delta^2 p_0}$；
    4. 检查 $|\hat{p_0} - p_0|$ 是否满足精度，满足则结束迭代；
    5. 令 $p_0 = \hat{p_0}$.

</div>
