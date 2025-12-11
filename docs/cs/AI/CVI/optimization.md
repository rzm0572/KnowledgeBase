# Model fitting and Optimization

## Model fitting

对于一个满足 $b = f_x(a)$ 的数学模型，其中

- $a$ 是模型的输入
- $b$ 是模型的输出
- $x$ 是模型的参数

我们希望通过调整参数 $x$，使得模型在给定的一组输入输出样本集 $\{(a_i, b_i)\}_{i=1}^N$ 上表现良好，即对于每个样本 $(a_i, b_i)$，都有 $b_i \approx f_{x}(a_i)$，这种通过数据集来估计模型参数的方法被称为学习（learning）或训练（training）。

为了衡量模型在样本集上的表现，我们通常会定义一个损失函数（loss function） $L(b, \hat{b})$，用于度量模型输出 $\hat{b}$ 与真实输出 $b$ 之间的差异：

- 均方误差（mean squared error，MSE）：使用平方项来衡量残差的大小

    $$
        \hat{x} = \underset{x}{\operatorname{argmin}} \sum_{i} \left( b_i - a_i^{\mathrm{T}} x \right)^2. 
    $$

- 极大似然估计（maximum likelihood estimation，MLE）：

    将所有数据点视为一系列独立同分布的随机变量的取值，我们希望通过调整模型参数使得模型取得当前数据点的概率最大化。例如，假设误差满足高斯分布，即

    $$
        b_i = a_i^{\mathrm{T}} x + r, \enspace r \sim \mathcal{N}(0, \sigma^2)
    $$

    我们可以求出在模型参数为 $x$ 的情况下第 $i$ 个数据点为 $(a_i, b_i)$ 的条件概率：

    $$
        \mathbb{P} \left[ (a_i, b_i) \mid x \right] = \mathbb{P} \left[ r = b_i - a_i^{\mathrm{T}} x \right] \propto \exp \left[ -\frac{1}{2 \sigma^2} (b_i - a_i^{\mathrm{T}} x)^2 \right].
    $$

    由于所有数据点的残差独立同分布，我们可以得到总的概率：

    $$
        \mathbb{P} \left[ (a_1, b_1)(a_2, b_2) \ldots (a_n, b_n) \mid x \right] = \prod_{i=1}^n \mathbb{P} \left[ (a_i, b_i) \mid x \right]
        \propto \exp \left[ -\frac{1}{2 \sigma^2} \left\| \mathbf{b} - A \mathbf{x} \right\|_2^2 \right].
    $$

    由此我们便得到了高斯分布下极大似然估计的公式：

    $$
        \hat{x} = \underset{x}{\operatorname{argmax}} \mathbb{P} \left[ (a_1, b_1)(a_2, b_2)\ldots(a_n, b_n) \mid x \right]
        = \underset{x}{\operatorname{argmin}} \left\| \mathbf{b} - A \mathbf{x} \right\|_2^2.
    $$

    可以发现，在误差满足高斯分布的情况下，极大似然估计的结果与均方误差的结果是相同的。


## Solve the optimization problem    

通过损失函数，我们将模型拟合问题转化为了最优化问题，下一步便是求解这个最优化问题。

部分最优化问题具有比较良好的性质，例如使用 MSE 优化线性模型存在解析解，其解为线性方程组

$$
    A^{\mathrm{T}} A x = A^{\mathrm{T}} b
$$

的解，然而更多的最优化问题不存在解析解，因此我们需要使用数值方法来求解。


下降法是一种常用的求解最优化问题的数值算法，其基本流程为：

```
x = x_0
while not converged:
    h = descending_direction(x)
    alpha = descending_step(x, h)
    x = x + alpha * h
```

其中，$x_0$ 是初始值，$h$ 是下降方向，$\alpha$ 是步长，在每轮迭代中我们需要求出最佳的 $h$ 和 $\alpha$。

- Steepest Descent Method
- Newton Method
- Gauss-Newton Method
- Levenberg-Marquardt Method

## Robustness of model fitting

Outliers 是数据集中与我们的假设差距很大的点，会对模型拟合效果产生巨大的影响。例如在 MSE 损失函数下，由于损失函数的值与残差的大小呈平方关系，因此异常值对模型参数的影响远大于正常值。

为了解决这个问题，我们希望可以减小 Outliers 对损失函数取值的影响，例如使用 L1 损失函数或 Huber 损失函数：

$$
    L_1(b, \hat{b}) = \sum_{i} \left| b_i - \hat{b}_i \right| \\
    \operatorname{Huber}(b, \hat{b}) = \left\{ \begin{array}{ll} \frac{1}{2} (b_i - \hat{b}_i)^2 & |b_i - \hat{b}_i| \leq \delta \\ \delta (|b_i - \hat{b}_i| - \frac{\delta}{2}) & |b_i - \hat{b}_i| > \delta \end{array} \right.
$$

Huber 损失函数解决了 MSE 损失函数受异常值影响大的缺点，也避免了 L1 损失函数在 0 处不光滑的问题。

除了选择不同的损失函数，我们也可以使用 RANSAC（随机采样一致性算法）来处理异常值。RANSAC 算法的基本思路是：

1. 随机选取一部分数据 $P_i$ 作为初始样本集
2. 根据这些样本集估计模型参数
3. 计算剩余数据集中每个点到模型的距离，计算距离小于阈值的数据点集合 $Q_i$
4. 重复上述步骤，找到使得距离小于阈值的数据点数最大的采样点集，i.e. $P_k = \underset{i}{\operatorname{argmax}} |Q_i|$.
5. 对 $P_k \cup Q_k$ 做拟合，得到最终的模型参数

当模型参数量较大时，RANSAC 需要大量采样，因此效率较低。


## Overfitting and Underfitting

如果参数过多，模型会过于复杂，将噪声等信息全部包含在内，导致泛化能力差，即出现过拟合（overfitting）现象。如果参数过少，模型会过于简单，无法拟合训练数据，即出现欠拟合（underfitting）现象。

![pZ93cef.md.png](https://s21.ax1x.com/2025/11/09/pZ93cef.md.png)

Ill-posed problem：假设的模型复杂度过高，导致模型存在多个最优解。

在优化之前确定模型的复杂度是比较难的，我们希望可以在优化过程中确定模型的复杂度。Regularization（正则化）是一种常用的方法，通过限制模型的复杂度来防止过拟合。

假设我们需要拟合的模型的损失函数为 $L_x(A, \hat{b})$，其中 $A$ 是输入矩阵，$\hat{b}$ 是输出向量，$x$ 是模型参数。为了限制模型的复杂度，我们希望 $x$ 中对结果影响较小的分量在优化过程中收敛到 0。为了做到这一点，我们就需要限制 $\left\| x \right\|$ 的大小：

- $L_2$ regularization：限制 $\left\| x \right\|_2 \leqslant 1$
- $L_1$ regularization：限制 $\left\| x \right\|_1 \leqslant 1$
    - 可以让解变得稀疏

除了将正则项作为约束条件的做法，我们也可以将正则项加到目标函数中。

